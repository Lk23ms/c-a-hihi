# Bài 3: Chia Để Trị (Divide and Conquer)

## 1. Ý tưởng cốt lõi

Chia để trị gồm 3 bước:
1. **Chia (Divide):** chia bài toán lớn thành các bài toán con nhỏ hơn, giống hệt bài toán gốc nhưng kích thước nhỏ hơn.
2. **Trị (Conquer):** giải từng bài toán con (thường bằng cách gọi đệ quy).
3. **Kết hợp (Combine):** ghép kết quả các bài toán con lại thành đáp án cho bài toán lớn.

**Ví dụ dễ hình dung trước khi vào code:** giống như bạn cần đếm số học sinh trong cả một trường học lớn. Thay vì đếm từng em một, hiệu trưởng chia việc: mỗi lớp tự đếm số học sinh của lớp mình (bài toán con nhỏ hơn), rồi các lớp báo cáo số liệu lên, hiệu trưởng chỉ cần cộng tổng lại (kết hợp) — nhanh hơn nhiều so với tự đếm hết cả trường một mình.

## 2. Ví dụ nền tảng: Merge Sort (sắp xếp trộn)

```cpp
void tron(vector<int> &a, int trai, int giua, int phai) {
    vector<int> tam;
    int i = trai, j = giua + 1;

    while (i <= giua && j <= phai) {
        if (a[i] <= a[j]) tam.push_back(a[i++]);
        else tam.push_back(a[j++]);
    }
    while (i <= giua) tam.push_back(a[i++]);
    while (j <= phai) tam.push_back(a[j++]);

    for (int k = 0; k < tam.size(); k++) {
        a[trai + k] = tam[k];
    }
}

void mergeSort(vector<int> &a, int trai, int phai) {
    if (trai >= phai) return; // base case: đoạn chỉ có 1 phần tử, đã "sắp xếp" xong

    int giua = (trai + phai) / 2;
    mergeSort(a, trai, giua);      // Chia: sắp xếp nửa trái
    mergeSort(a, giua + 1, phai);  // Chia: sắp xếp nửa phải
    tron(a, trai, giua, phai);     // Kết hợp: trộn 2 nửa đã sắp xếp lại với nhau
}
```

**Giải thích:** `mergeSort` chia mảng làm đôi liên tục cho tới khi mỗi đoạn chỉ còn 1 phần tử (chắc chắn đã "sắp xếp" vì chỉ có 1 phần tử), sau đó hàm `tron` ghép 2 đoạn đã sắp xếp lại thành 1 đoạn lớn hơn cũng được sắp xếp, cứ thế "leo ngược" lên tới khi ghép được toàn bộ mảng.

**Ví dụ dễ hình dung:** giống việc trộn 2 bộ bài đã được xếp sẵn theo thứ tự (bộ A và bộ B), bạn chỉ cần liên tục so sánh lá bài trên cùng của mỗi bộ, lấy lá nhỏ hơn đặt xuống trước — không cần xáo trộn lại từ đầu, vì cả 2 bộ ban đầu đã được sắp xếp sẵn rồi.

**Độ phức tạp:** O(n log n) — nhanh hơn nhiều so với Bubble Sort O(n²) khi `n` lớn, đây là lý do Merge Sort (và các thuật toán sắp xếp O(n log n) khác) được dùng trong hàm `sort()` có sẵn của C++.

## 3. Ví dụ thứ hai: Tìm số lớn nhất bằng chia để trị

```cpp
int timMax(vector<int> &a, int trai, int phai) {
    if (trai == phai) return a[trai]; // base case: chỉ 1 phần tử

    int giua = (trai + phai) / 2;
    int maxTrai = timMax(a, trai, giua);       // Chia + Trị: max của nửa trái
    int maxPhai = timMax(a, giua + 1, phai);   // Chia + Trị: max của nửa phải

    return max(maxTrai, maxPhai); // Kết hợp: max tổng thể là max của 2 nửa
}
```

Bài này Brute Force (duyệt thẳng O(n)) đã đủ nhanh rồi, không cần chia để trị — nhưng ví dụ này giúp thấy rõ cấu trúc 3 bước Chia-Trị-Kết hợp một cách đơn giản nhất trước khi áp dụng vào bài phức tạp hơn như Merge Sort.

## 4. Ví dụ thực tế: Tìm ngày doanh thu cao nhất trong khoảng thời gian dài

Một cửa hàng ghi lại doanh thu mỗi ngày trong cả năm, muốn nhanh chóng biết ngày nào doanh thu cao nhất trong 1 khoảng thời gian tùy chọn (có thể hỏi nhiều lần với các khoảng khác nhau) — áp dụng ý tưởng chia để trị để xử lý hiệu quả:

```cpp
#include <iostream>
#include <vector>
using namespace std;

int timNgayDoanhThuCaoNhat(vector<int> &doanhThu, int trai, int phai) {
    if (trai == phai) return trai; // chỉ 1 ngày, chính nó là ngày cao nhất

    int giua = (trai + phai) / 2;
    int ngayMaxTrai = timNgayDoanhThuCaoNhat(doanhThu, trai, giua);
    int ngayMaxPhai = timNgayDoanhThuCaoNhat(doanhThu, giua + 1, phai);

    return (doanhThu[ngayMaxTrai] >= doanhThu[ngayMaxPhai]) ? ngayMaxTrai : ngayMaxPhai;
}

int main() {
    vector<int> doanhThu = {5, 12, 8, 20, 3, 15, 9}; // doanh thu 7 ngày, đơn vị triệu đồng

    int ngayCaoNhat = timNgayDoanhThuCaoNhat(doanhThu, 0, doanhThu.size() - 1);

    cout << "Ngay doanh thu cao nhat: ngay " << (ngayCaoNhat + 1)
         << ", doanh thu: " << doanhThu[ngayCaoNhat] << " trieu dong" << endl;

    return 0;
}
```

Với bài toán đơn giản này, chia để trị không nhanh hơn duyệt thẳng, nhưng cấu trúc "chia nhỏ, giải từng phần, ghép lại" chính là nền tảng cho các cấu trúc dữ liệu mạnh mẽ hơn như Segment Tree (sẽ học ở bài Cấu trúc dữ liệu nâng cao) — nơi ý tưởng chia để trị giúp trả lời rất nhiều truy vấn (query) một cách cực nhanh.

## Bài tập gợi ý

1. Cài đặt hoàn chỉnh Merge Sort và thử chạy với mảng 10 số ngẫu nhiên, in ra từng bước chia và trộn để hiểu rõ quá trình.
2. Dùng chia để trị viết hàm tính tổng các phần tử trong mảng (chia đôi, tính tổng từng nửa, cộng lại).
3. Cài đặt thuật toán Quick Sort (một thuật toán chia để trị khác, chọn 1 phần tử làm "chốt" (pivot) để chia mảng thành 2 phần nhỏ hơn/lớn hơn chốt).
4. So sánh thời gian chạy thực tế giữa Bubble Sort O(n²) và Merge Sort O(n log n) trên mảng 10,000 phần tử ngẫu nhiên.
5. (Nâng cao nhẹ) Bài toán "đếm số cặp nghịch thế" (inversion count — đếm số cặp (i, j) với i < j nhưng a[i] > a[j]) có thể giải bằng chia để trị, tận dụng luôn quá trình trộn của Merge Sort để đếm số cặp nghịch thế mà không cần vòng lặp O(n²) riêng. Tìm hiểu và thử cài đặt.
