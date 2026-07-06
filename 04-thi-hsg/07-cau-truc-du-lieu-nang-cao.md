# Bài 7: Cấu Trúc Dữ Liệu Nâng Cao — DSU, Segment Tree, BIT

## 1. DSU (Disjoint Set Union) — Union-Find

DSU giúp trả lời nhanh 2 câu hỏi: "2 phần tử này có cùng nhóm không?" và "gộp 2 nhóm lại làm 1".

```cpp
int cha[100005];

void khoiTao(int n) {
    for (int i = 1; i <= n; i++) cha[i] = i; // ban đầu, mỗi phần tử tự làm nhóm trưởng của chính mình
}

int timNhomTruong(int x) {
    if (cha[x] == x) return x;
    return cha[x] = timNhomTruong(cha[x]); // "nén đường" - giúp các lần tìm sau nhanh hơn
}

void gopNhom(int x, int y) {
    int nhomX = timNhomTruong(x);
    int nhomY = timNhomTruong(y);
    if (nhomX != nhomY) {
        cha[nhomX] = nhomY; // gộp 2 nhóm lại làm 1
    }
}

bool cungNhom(int x, int y) {
    return timNhomTruong(x) == timNhomTruong(y);
}
```

**Giải thích:** mỗi phần tử có 1 "cha" — nếu cha của `x` chính là `x`, `x` là "nhóm trưởng" của cả nhóm mình. `timNhomTruong` đi ngược lên tới tận nhóm trưởng, và "nén đường" bằng cách gán thẳng `cha[x]` về nhóm trưởng luôn để lần sau tra cứu nhanh hơn.

**Ví dụ dễ hình dung:** DSU giống hệt hệ thống **quản lý các nhóm bạn học chung dự án**. Ban đầu mỗi bạn tự đứng riêng lẻ (nhóm trưởng của chính mình). Khi 2 bạn quyết định làm chung nhóm, một trong hai trở thành nhóm trưởng đại diện cho cả 2. Khi hỏi "bạn A và bạn B có cùng nhóm không?", chỉ cần tra xem cả 2 có cùng chung 1 "nhóm trưởng cao nhất" hay không.

**Ứng dụng phổ biến:** kiểm tra đồ thị có chu trình không, xây cây khung nhỏ nhất (thuật toán Kruskal đã nhắc ở bài Graph), nhóm các phần tử theo quan hệ tương đương.

## 2. Segment Tree (cây phân đoạn)

Segment Tree giúp trả lời nhanh các truy vấn trên 1 đoạn của mảng (như "tổng từ vị trí l đến r"), và **cập nhật** giá trị 1 phần tử — cả 2 thao tác đều chỉ mất O(log n) thay vì O(n).

```cpp
int cay[400005]; // cây segment tree, kích thước ~4 lần mảng gốc
int a[100005];

void xayDung(int node, int start, int end) {
    if (start == end) {
        cay[node] = a[start];
        return;
    }
    int mid = (start + end) / 2;
    xayDung(node * 2, start, mid);         // xây nhánh trái
    xayDung(node * 2 + 1, mid + 1, end);   // xây nhánh phải
    cay[node] = cay[node * 2] + cay[node * 2 + 1]; // node cha = tổng 2 con
}

int truyVanTong(int node, int start, int end, int l, int r) {
    if (r < start || end < l) return 0; // đoạn truy vấn không giao với đoạn hiện tại
    if (l <= start && end <= r) return cay[node]; // đoạn hiện tại nằm gọn trong truy vấn

    int mid = (start + end) / 2;
    int tongTrai = truyVanTong(node * 2, start, mid, l, r);
    int tongPhai = truyVanTong(node * 2 + 1, mid + 1, end, l, r);
    return tongTrai + tongPhai;
}
```

**Ví dụ dễ hình dung:** Segment Tree giống như **sơ đồ tổ chức 1 công ty theo cấp bậc** để nhanh chóng tính tổng doanh số cả công ty hoặc từng phòng ban. Thay vì cộng doanh số của mọi nhân viên mỗi lần cần biết tổng (rất chậm), mỗi "trưởng nhóm nhỏ" đã tổng hợp sẵn doanh số của nhóm mình, trưởng phòng tổng hợp từ các trưởng nhóm, giám đốc tổng hợp từ các trưởng phòng — khi cần biết tổng doanh số 1 phòng ban bất kỳ, chỉ cần hỏi đúng vài cấp quản lý liên quan thay vì hỏi từng nhân viên.

## 3. BIT / Fenwick Tree (cây chỉ số nhị phân)

BIT là một cấu trúc gọn nhẹ hơn Segment Tree, cũng giải bài toán "tổng đoạn + cập nhật" nhưng code ngắn hơn nhiều (dù khó hình dung cơ chế bên trong hơn):

```cpp
int bit[100005];
int n;

void capNhat(int i, int giaTri) {
    for (; i <= n; i += i & (-i)) {
        bit[i] += giaTri;
    }
}

int truyVanTongDau(int i) { // tổng từ phần tử 1 tới i
    int tong = 0;
    for (; i > 0; i -= i & (-i)) {
        tong += bit[i];
    }
    return tong;
}

int truyVanTongDoan(int l, int r) { // tổng từ l tới r
    return truyVanTongDau(r) - truyVanTongDau(l - 1);
}
```

**Giải thích:** phép toán `i & (-i)` (dùng phép AND bit) tách ra "bit cuối cùng khác 0" của `i`, giúp BIT nhảy qua các "khối" chỉ số một cách thông minh. Cơ chế nội tại của BIT khá trừu tượng — điều quan trọng khi mới học là hiểu **BIT dùng để làm gì** (tổng đoạn + cập nhật nhanh) trước khi đào sâu tại sao nó hoạt động đúng.

## 4. So sánh nhanh 3 cấu trúc

| Cấu trúc | Dùng khi nào | Độ khó cài đặt |
|---|---|---|
| DSU | Nhóm phần tử, kiểm tra cùng nhóm | Dễ |
| Segment Tree | Truy vấn đoạn phức tạp (tổng, max, min...), cập nhật | Trung bình |
| BIT | Chỉ cần tổng đoạn + cập nhật, muốn code ngắn gọn | Trung bình (nhưng khó hiểu cơ chế) |

## 5. Ví dụ thực tế: Theo dõi điểm tích lũy học sinh theo thời gian thực

Một hệ thống lớp học trực tuyến cần cập nhật điểm cộng cho học sinh liên tục trong suốt buổi học, và luôn có thể tra cứu nhanh "tổng điểm của học sinh từ số thứ tự `l` đến `r` trong danh sách lớp" — đây chính là bài toán Segment Tree/BIT kinh điển:

```cpp
#include <iostream>
using namespace std;

int bit[1005];
int n;

void capNhat(int i, int giaTri) {
    for (; i <= n; i += i & (-i)) bit[i] += giaTri;
}

int truyVanTongDau(int i) {
    int tong = 0;
    for (; i > 0; i -= i & (-i)) tong += bit[i];
    return tong;
}

int main() {
    cout << "So hoc sinh trong lop: ";
    cin >> n;

    cout << "He thong san sang. Giao vien co the:\n";
    cout << "1. Cong diem cho 1 hoc sinh (cap nhat)\n";
    cout << "2. Tra tong diem 1 khoang hoc sinh (truy van)\n";

    // Ví dụ: cộng 5 điểm cho học sinh số 3
    capNhat(3, 5);
    // Ví dụ: cộng 3 điểm cho học sinh số 7
    capNhat(7, 3);

    // Tra tổng điểm từ học sinh 1 đến 10
    cout << "Tong diem hoc sinh 1-10: " << truyVanTongDau(10) << endl;

    return 0;
}
```

Nhờ BIT, dù lớp học có hàng nghìn học sinh và giáo viên cộng điểm liên tục hàng trăm lần trong buổi học, mỗi thao tác cập nhật/tra cứu vẫn cực nhanh (O(log n)) thay vì phải cộng lại từ đầu mỗi lần (O(n)).

## Bài tập gợi ý

1. Cài đặt DSU hoàn chỉnh, dùng để kiểm tra 1 đồ thị cho trước có chu trình hay không (gợi ý: nếu thêm 1 cạnh mà 2 đỉnh đã cùng nhóm từ trước, tức là đã tạo thành chu trình).
2. Cài đặt Segment Tree cho bài toán tìm giá trị LỚN NHẤT trong đoạn `[l, r]` (thay vì tổng như ví dụ trên).
3. Cài đặt BIT hoàn chỉnh và giải bài toán: cho mảng `n` số, hỗ trợ 2 loại truy vấn "cộng thêm giá trị vào 1 phần tử" và "tính tổng đoạn [l, r]", với hàng nghìn truy vấn liên tiếp.
4. So sánh thời gian chạy giữa cách tính tổng đoạn thông thường (vòng lặp O(n) mỗi lần hỏi) và dùng BIT (O(log n) mỗi lần hỏi) khi có 100,000 truy vấn trên mảng 100,000 phần tử.
5. (Nâng cao nhẹ) Tìm hiểu kỹ thuật "Lazy Propagation" trong Segment Tree — dùng khi cần cập nhật giá trị cho CẢ MỘT ĐOẠN (không chỉ 1 phần tử) một cách hiệu quả, ví dụ "cộng thêm 5 điểm cho tất cả học sinh từ số 10 đến số 30".
