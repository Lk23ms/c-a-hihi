# Bài 6: STL Cơ Bản — vector, pair, string nâng cao

STL (Standard Template Library) là thư viện có sẵn của C++, cung cấp các cấu trúc dữ liệu và thuật toán dựng sẵn — giúp code ngắn gọn, ít lỗi hơn nhiều so với tự viết mảng/thuật toán từ đầu.

## 1. `vector` — mảng có thể thay đổi kích thước

Nhược điểm lớn nhất của mảng thường (`int a[100]`) là kích thước cố định. `vector` giải quyết vấn đề này:

```cpp
#include <vector>
using namespace std;

vector<int> v; // vector rỗng, có thể thêm phần tử dần dần

v.push_back(10); // thêm 10 vào cuối
v.push_back(20);
v.push_back(30);

cout << v.size() << endl;  // 3 - số phần tử hiện có
cout << v[0] << endl;      // 10 - truy cập như mảng thường

v.pop_back();               // xóa phần tử cuối -> còn {10, 20}
```

**Ví dụ dễ hình dung:** mảng thường giống như một cái hộp đựng bút cố định 10 ngăn — mua về là 10 ngăn mãi mãi, dùng ít cũng phí, cần nhiều hơn cũng chịu. `vector` giống như một cái túi vải co giãn — bỏ thêm bút vào (`push_back`) túi tự phình to ra, lấy bớt bút ra (`pop_back`) túi tự nhỏ lại, không bao giờ lo thiếu hay thừa chỗ chứa.

### Khởi tạo vector với giá trị có sẵn
```cpp
vector<int> v = {1, 2, 3, 4, 5};
vector<int> v2(10, 0); // vector gồm 10 phần tử, tất cả giá trị đều là 0
```

### Duyệt vector
```cpp
for (int i = 0; i < v.size(); i++) {
    cout << v[i] << " ";
}

// hoặc gọn hơn với range-based for
for (int x : v) {
    cout << x << " ";
}
```

## 2. `pair` — gộp 2 giá trị lại thành 1

```cpp
#include <utility> // hoặc chỉ cần <bits/stdc++.h> là đủ mọi thư viện

pair<string, int> hocSinh = {"Khoi", 16}; // ghép tên và tuổi lại

cout << hocSinh.first << endl;   // "Khoi" - phần tử đầu
cout << hocSinh.second << endl;  // 16 - phần tử thứ hai
```

**Ví dụ dễ hình dung:** `pair` giống như 1 chiếc vòng tay đôi bạn thân — mỗi chiếc luôn đi thành cặp, không tách rời. `hocSinh.first` và `hocSinh.second` là 2 nửa của cùng 1 cặp, luôn dùng chung với nhau.

### Vector chứa pair — rất hay dùng để lưu danh sách kèm thông tin đi kèm
```cpp
vector<pair<string, int>> danhSach;
danhSach.push_back({"Khoi", 16});
danhSach.push_back({"Lan", 15});

for (auto p : danhSach) {
    cout << p.first << " - " << p.second << " tuoi" << endl;
}
```

## 3. `string` nâng cao — vài hàm hữu ích thêm

```cpp
string s = "  Hello World  ";

// Nối nhiều chuỗi
string ho = "Nguyen", ten = "Khoi";
string hoTen = ho + " " + ten;

// Đảo ngược chuỗi (dùng hàm có sẵn thay vì tự viết vòng lặp)
#include <algorithm>
reverse(s.begin(), s.end());

// Sắp xếp ký tự trong chuỗi theo thứ tự bảng chữ cái
sort(s.begin(), s.end());
```

## 4. Các thuật toán STL hay dùng chung với vector

```cpp
#include <algorithm>

vector<int> v = {5, 2, 8, 1, 9};

sort(v.begin(), v.end());               // sắp xếp tăng dần
reverse(v.begin(), v.end());            // đảo ngược thứ tự
int maxVal = *max_element(v.begin(), v.end()); // tìm giá trị lớn nhất
int minVal = *min_element(v.begin(), v.end()); // tìm giá trị nhỏ nhất
int tong = accumulate(v.begin(), v.end(), 0);  // tính tổng, bắt đầu từ 0
```

> **Mẹo thi HSG:** hầu như mọi bài thi lập trình đều nên dùng `vector` thay vì mảng tĩnh thô, và tận dụng tối đa các hàm có sẵn trong `<algorithm>` — vừa nhanh, vừa ít lỗi hơn tự viết tay.

## 5. Ví dụ thực tế: Quản lý giỏ hàng mua sắm online

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <numeric>
using namespace std;

int main() {
    vector<pair<string, int>> gioHang; // mỗi món hàng: {tên, giá tiền}

    int n;
    cout << "So mon hang muon them vao gio: ";
    cin >> n;

    for (int i = 0; i < n; i++) {
        string ten;
        int gia;
        cout << "Ten mon hang va gia tien: ";
        cin >> ten >> gia;
        gioHang.push_back({ten, gia});
    }

    // Tính tổng tiền cần thanh toán
    int tongTien = 0;
    for (auto mon : gioHang) {
        tongTien += mon.second;
    }

    cout << "\n--- Gio hang cua ban ---" << endl;
    for (auto mon : gioHang) {
        cout << "- " << mon.first << ": " << mon.second << " dong" << endl;
    }
    cout << "Tong tien can thanh toan: " << tongTien << " dong" << endl;

    return 0;
}
```

Đây gần như y hệt cách một trang thương mại điện tử thực tế lưu trữ giỏ hàng của bạn: mỗi món hàng là 1 `pair` (tên + giá), toàn bộ giỏ hàng là 1 `vector` có thể thêm/bớt món linh hoạt.

## Bài tập gợi ý

1. Viết chương trình dùng `vector<int>` để nhập `n` số, sau đó in ra số lớn nhất, nhỏ nhất, và tổng — dùng các hàm `max_element`, `min_element`, `accumulate` trong `<algorithm>`/`<numeric>`.
2. Dùng `vector<pair<string, double>>` lưu danh sách học sinh kèm điểm trung bình, sắp xếp danh sách theo điểm giảm dần bằng `sort` với hàm so sánh tùy chỉnh.
3. Viết chương trình loại bỏ các phần tử trùng lặp trong 1 `vector<int>` (gợi ý: có thể sắp xếp trước rồi dùng hàm `unique` có sẵn trong `<algorithm>`).
4. Cho một chuỗi, đếm số lần xuất hiện của mỗi ký tự bằng cách dùng `vector<pair<char, int>>` hoặc mảng đếm.
5. (Nâng cao nhẹ) Viết chương trình quản lý danh sách công việc cần làm (to-do list) đơn giản trong 1 phiên chạy chương trình: cho phép thêm việc (`push_back`), đánh dấu hoàn thành (xóa khỏi vector), và in ra danh sách còn lại — tất cả dùng `vector<string>`.
