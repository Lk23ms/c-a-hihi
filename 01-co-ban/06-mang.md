# Bài 6: Mảng (Array) 1 Chiều & 2 Chiều

## 1. Mảng 1 chiều là gì?

Mảng là tập hợp các phần tử **cùng kiểu dữ liệu**, lưu liên tiếp trong bộ nhớ, truy cập qua chỉ số (index bắt đầu từ 0).

```cpp
int a[5]; // mảng 5 số nguyên, chưa khởi tạo giá trị

int b[5] = {10, 20, 30, 40, 50}; // khởi tạo luôn giá trị

int c[] = {1, 2, 3}; // không cần ghi số lượng, trình biên dịch tự đếm
```

Truy cập phần tử:
```cpp
cout << b[0] << endl; // 10 (phần tử đầu tiên)
cout << b[4] << endl; // 50 (phần tử cuối, vì mảng có 5 phần tử: chỉ số 0-4)
b[2] = 99; // gán lại giá trị
```

⚠️ **Lỗi cực kỳ phổ biến:** truy cập chỉ số ngoài phạm vi (ví dụ `b[5]` khi mảng chỉ có 5 phần tử, chỉ số hợp lệ 0-4) → gây lỗi khó phát hiện (undefined behavior), C++ **không tự kiểm tra** lỗi này.

## 2. Duyệt mảng bằng vòng lặp

```cpp
int n = 5;
int a[100]; // khai báo dư để chứa tối đa 100 phần tử

for (int i = 0; i < n; i++) {
    cin >> a[i]; // nhập từng phần tử
}

for (int i = 0; i < n; i++) {
    cout << a[i] << " "; // in từng phần tử
}
```

## 3. Kích thước mảng cố định (mảng tĩnh)

Trong thi HSG, thường khai báo mảng với kích thước tối đa cho phép theo đề bài:
```cpp
const int MAXN = 100005;
int a[MAXN];
```

## 4. Các thao tác thường gặp trên mảng

### Tìm giá trị lớn nhất / nhỏ nhất
```cpp
int maxVal = a[0], minVal = a[0];
for (int i = 1; i < n; i++) {
    if (a[i] > maxVal) maxVal = a[i];
    if (a[i] < minVal) minVal = a[i];
}
```

### Tính tổng, trung bình
```cpp
long long tong = 0;
for (int i = 0; i < n; i++) tong += a[i];
double trungBinh = (double)tong / n;
```

### Đảo ngược mảng
```cpp
for (int i = 0; i < n / 2; i++) {
    swap(a[i], a[n - 1 - i]); // hàm swap có sẵn trong <algorithm>
}
```

### Sắp xếp mảng (dùng hàm có sẵn — chi tiết ở bài Sắp xếp Trung cấp)
```cpp
#include <algorithm>
sort(a, a + n); // sắp xếp tăng dần
```

## 5. Mảng 2 chiều (ma trận)

```cpp
int b[3][4]; // ma trận 3 hàng, 4 cột

int c[2][3] = {
    {1, 2, 3},
    {4, 5, 6}
};
```

Duyệt mảng 2 chiều dùng vòng lặp lồng nhau:
```cpp
int hang = 3, cot = 4;
int a[10][10];

for (int i = 0; i < hang; i++) {
    for (int j = 0; j < cot; j++) {
        cin >> a[i][j];
    }
}

for (int i = 0; i < hang; i++) {
    for (int j = 0; j < cot; j++) {
        cout << a[i][j] << " ";
    }
    cout << endl;
}
```

## 6. Mảng làm tham số hàm

```cpp
// Khi truyền mảng vào hàm, thực chất truyền địa chỉ -> hàm có thể sửa mảng gốc
void inMang(int a[], int n) {
    for (int i = 0; i < n; i++) cout << a[i] << " ";
    cout << endl;
}

int main() {
    int arr[5] = {1, 2, 3, 4, 5};
    inMang(arr, 5); // luôn phải truyền kèm kích thước n
}
```

## 7. Ví dụ thực tế: Tìm bạn cao điểm nhất lớp

Cô giáo muốn biết ai đạt điểm cao nhất lớp trong bài kiểm tra vừa rồi, và điểm trung bình cả lớp là bao nhiêu:

```cpp
#include <iostream>
using namespace std;

int main() {
    int soHocSinh;
    cout << "So hoc sinh: ";
    cin >> soHocSinh;

    double diem[100];
    double tong = 0;
    int viTriCaoNhat = 0;

    for (int i = 0; i < soHocSinh; i++) {
        cout << "Diem hoc sinh " << (i + 1) << ": ";
        cin >> diem[i];
        tong += diem[i];

        if (diem[i] > diem[viTriCaoNhat]) {
            viTriCaoNhat = i;
        }
    }

    cout << "Diem trung binh ca lop: " << tong / soHocSinh << endl;
    cout << "Hoc sinh so " << (viTriCaoNhat + 1) << " co diem cao nhat: " << diem[viTriCaoNhat] << endl;

    return 0;
}
```

Đây là bài toán rất thực tế: mảng `diem[]` đóng vai trò như một cuốn sổ điểm, mình duyệt qua từng học sinh (giống cô giáo lật từng trang sổ), vừa cộng dồn tính tổng, vừa so sánh để tìm điểm cao nhất — chỉ cần 1 lượt duyệt mảng là xong.

## Ví dụ thực tế 2: Tìm phần tử xuất hiện nhiều nhất

```cpp
#include <iostream>
using namespace std;

int main() {
    int n;
    cin >> n;
    int a[1005], dem[1005] = {0}; // giả sử giá trị phần tử từ 0-1000

    for (int i = 0; i < n; i++) {
        cin >> a[i];
        dem[a[i]]++;
    }

    int maxDem = 0, giaTri = -1;
    for (int i = 0; i < n; i++) {
        if (dem[a[i]] > maxDem) {
            maxDem = dem[a[i]];
            giaTri = a[i];
        }
    }

    cout << "Gia tri xuat hien nhieu nhat: " << giaTri
         << " (" << maxDem << " lan)" << endl;
    return 0;
}
```

## Bài tập gợi ý

1. Nhập mảng `n` số nguyên, in ra mảng đã đảo ngược thứ tự.
2. Nhập mảng `n` số nguyên, tính tổng các số chẵn và tổng các số lẻ riêng biệt.
3. Kiểm tra một mảng có phải là mảng đối xứng (palindrome) hay không (ví dụ: `1 2 3 2 1`).
4. Nhập ma trận vuông `n x n`, tính tổng đường chéo chính và đường chéo phụ.
5. (Nâng cao nhẹ) Cho mảng `n` số nguyên, tìm dãy con liên tiếp có tổng lớn nhất (bài toán Maximum Subarray — gợi ý: đây là bài toán kinh điển trong thi HSG, có thể giải bằng thuật toán Kadane với độ phức tạp O(n), sẽ học kỹ hơn ở phần Quy hoạch động).
