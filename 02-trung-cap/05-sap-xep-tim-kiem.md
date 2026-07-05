# Bài 5: Sắp Xếp & Tìm Kiếm Cơ Bản

## 1. Sắp xếp nổi bọt (Bubble Sort) — hiểu cơ chế trước khi dùng hàm có sẵn

```cpp
void bubbleSort(int a[], int n) {
    for (int i = 0; i < n - 1; i++) {
        for (int j = 0; j < n - 1 - i; j++) {
            if (a[j] > a[j + 1]) {
                swap(a[j], a[j + 1]); // đổi chỗ nếu sai thứ tự
            }
        }
    }
}
```

**Giải thích:** thuật toán so sánh từng cặp phần tử liền kề, nếu thấy sai thứ tự (phần tử trước lớn hơn phần tử sau) thì đổi chỗ. Lặp lại nhiều lượt cho tới khi cả mảng được sắp xếp.

**Ví dụ dễ hình dung:** giống như xếp hàng theo chiều cao — bạn đi dọc hàng, mỗi lần thấy 2 bạn đứng cạnh nhau mà bạn thấp hơn lại đứng trước bạn cao hơn, bạn nhắc 2 bạn đó đổi chỗ cho nhau. Đi hết hàng một lượt, quay lại đi tiếp lượt nữa — sau vài lượt, "bong bóng" bạn cao nhất sẽ dần "nổi" lên cuối hàng, giống hệt lý do gọi tên là Bubble Sort (nổi bọt).

⚠️ **Độ phức tạp:** Bubble Sort chạy khoảng `O(n²)` — với `n` lớn (vài chục nghìn trở lên) sẽ rất chậm. Trong thực tế thi đấu, gần như luôn dùng hàm `sort()` có sẵn thay vì tự viết Bubble Sort.

## 2. Dùng hàm `sort()` có sẵn — cách làm thực tế

```cpp
#include <algorithm>

int a[] = {5, 2, 8, 1, 9};
int n = 5;

sort(a, a + n);                        // sắp xếp tăng dần
sort(a, a + n, greater<int>());        // sắp xếp giảm dần
```

**Giải thích:** `sort(a, a + n)` nghĩa là "sắp xếp đoạn từ `a` (phần tử đầu) tới `a + n` (ngay sau phần tử cuối)". Đây là cách viết chuẩn của C++ khi làm việc với đoạn `[đầu, cuối)`.

Với `vector` (sẽ học kỹ ở bài sau), cách viết còn gọn hơn:
```cpp
vector<int> v = {5, 2, 8, 1, 9};
sort(v.begin(), v.end());
```

## 3. Sắp xếp theo tiêu chí tùy chỉnh

```cpp
bool soSanhGiam(int a, int b) {
    return a > b; // trả về true nếu a nên đứng trước b
}

int main() {
    int a[] = {5, 2, 8, 1, 9};
    sort(a, a + 5, soSanhGiam); // sắp xếp giảm dần theo hàm tự định nghĩa
}
```

Cách này rất hữu ích khi cần sắp xếp struct theo một field cụ thể (đã thấy ở bài Struct: sắp xếp học sinh theo điểm).

## 4. Tìm kiếm tuyến tính (Linear Search)

Duyệt từng phần tử cho tới khi tìm thấy — dùng được với mảng chưa sắp xếp:

```cpp
int timTuyenTinh(int a[], int n, int x) {
    for (int i = 0; i < n; i++) {
        if (a[i] == x) return i; // trả về vị trí tìm thấy
    }
    return -1; // không tìm thấy
}
```

**Ví dụ dễ hình dung:** giống như tìm 1 cuốn sách trong đống sách chưa sắp xếp — bạn phải lật từng cuốn một để kiểm tra, không có cách nào nhanh hơn nếu sách bị xáo trộn lung tung.

## 5. Tìm kiếm nhị phân (Binary Search) — chỉ dùng được khi mảng đã sắp xếp

```cpp
int timNhiPhan(int a[], int n, int x) {
    int trai = 0, phai = n - 1;

    while (trai <= phai) {
        int giua = (trai + phai) / 2;

        if (a[giua] == x) return giua;         // tìm thấy!
        else if (a[giua] < x) trai = giua + 1; // x nằm bên phải, thu hẹp về nửa phải
        else phai = giua - 1;                  // x nằm bên trái, thu hẹp về nửa trái
    }

    return -1; // không tìm thấy
}
```

**Ví dụ dễ hình dung:** giống như tra từ điển giấy — bạn không lật từng trang từ đầu, mà mở đại vào giữa cuốn từ điển, xem từ đó nằm trước hay sau trang mình vừa mở, rồi tiếp tục thu hẹp phạm vi tìm về một nửa mỗi lần. Cách này nhanh hơn tìm tuyến tính rất nhiều — với 1 triệu phần tử, tìm tuyến tính có thể cần tới 1 triệu bước, nhưng tìm nhị phân chỉ cần khoảng 20 bước!

> **Điều kiện bắt buộc:** mảng phải được **sắp xếp trước** thì tìm nhị phân mới hoạt động đúng — giống như từ điển phải được sắp theo bảng chữ cái thì bạn mới đoán được "mở đại vào giữa" là nên lật tới trước hay sau.

## 6. Ví dụ thực tế: Tra cứu điểm thi bằng số báo danh

Giả sử danh sách số báo danh đã được sắp xếp tăng dần sẵn (giống danh sách thi thực tế), học sinh muốn tra nhanh xem số báo danh của mình có trong danh sách trúng tuyển không:

```cpp
#include <iostream>
#include <algorithm>
using namespace std;

int main() {
    int n;
    cout << "So luong thi sinh trung tuyen: ";
    cin >> n;

    int sbd[100000];
    for (int i = 0; i < n; i++) cin >> sbd[i];

    sort(sbd, sbd + n); // đảm bảo đã sắp xếp trước khi tìm nhị phân

    int sbdCanTim;
    cout << "Nhap so bao danh can tra cuu: ";
    cin >> sbdCanTim;

    int viTri = timNhiPhan(sbd, n, sbdCanTim);

    if (viTri != -1)
        cout << "Chuc mung! Ban da trung tuyen." << endl;
    else
        cout << "Rat tiec, khong tim thay so bao danh nay." << endl;

    return 0;
}
```

Với danh sách trúng tuyển lên tới hàng trăm nghìn thí sinh, tìm nhị phân giúp tra cứu gần như tức thì thay vì phải chờ máy dò từng số một.

## Bài tập gợi ý

1. Tự viết thuật toán Selection Sort (chọn phần tử nhỏ nhất trong phần chưa sắp xếp, đưa nó về đúng vị trí, lặp lại).
2. Dùng `sort()` sắp xếp một mảng chuỗi (string) theo thứ tự bảng chữ cái.
3. Viết hàm tìm kiếm nhị phân trả về vị trí **đầu tiên** mà một số `x` xuất hiện, trong trường hợp mảng có nhiều phần tử trùng giá trị `x`.
4. So sánh thời gian chạy thực tế giữa tìm tuyến tính và tìm nhị phân trên mảng 1 triệu phần tử (gợi ý: dùng thư viện `<chrono>` để đo thời gian).
5. (Nâng cao nhẹ) Ứng dụng ý tưởng tìm nhị phân để giải bài toán khác thường gặp trong thi HSG: "tìm giá trị nhỏ nhất `x` sao cho một điều kiện P(x) nào đó là đúng" (kỹ thuật gọi là "Binary Search trên đáp án" — sẽ gặp lại kỹ thuật này nhiều ở Phần 4).
