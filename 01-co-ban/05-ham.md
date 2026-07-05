# Bài 5: Hàm (Function)

## 1. Vì sao cần dùng hàm?

Hàm giúp: tái sử dụng code, chia nhỏ bài toán lớn thành các phần dễ quản lý, và làm chương trình dễ đọc, dễ debug hơn.

## 2. Khai báo và định nghĩa hàm

```cpp
// Kiểu trả về    Tên hàm    Tham số
int tong(int a, int b) {
    return a + b;
}

int main() {
    int ketQua = tong(3, 5); // gọi hàm
    cout << ketQua << endl;  // 8
    return 0;
}
```

- Hàm không trả về giá trị nào thì dùng kiểu `void`.
```cpp
void chao() {
    cout << "Xin chao!" << endl;
    // không có return, hoặc "return;" (không có giá trị)
}
```

## 3. Khai báo nguyên mẫu (prototype)

Nếu hàm được định nghĩa **sau** `main()`, cần khai báo nguyên mẫu trước:

```cpp
#include <iostream>
using namespace std;

int binhPhuong(int x); // khai báo nguyên mẫu

int main() {
    cout << binhPhuong(5) << endl; // 25
    return 0;
}

int binhPhuong(int x) { // định nghĩa đầy đủ
    return x * x;
}
```

## 4. Tham số truyền theo giá trị vs tham chiếu

### Truyền theo giá trị (value) — mặc định, không thay đổi biến gốc
```cpp
void tang(int x) {
    x = x + 1; // chỉ thay đổi bản sao cục bộ
}

int main() {
    int a = 5;
    tang(a);
    cout << a << endl; // vẫn là 5
}
```

### Truyền theo tham chiếu (reference `&`) — thay đổi được biến gốc
```cpp
void tang(int &x) {
    x = x + 1; // thay đổi trực tiếp biến gốc
}

int main() {
    int a = 5;
    tang(a);
    cout << a << endl; // 6
}
```

> **Khi nào dùng tham chiếu?** Khi cần hàm trả về nhiều giá trị, hoặc khi truyền dữ liệu lớn (mảng, struct) để tránh sao chép tốn bộ nhớ — dùng `const &` nếu chỉ đọc, không sửa.

## 5. Hàm nạp chồng (overloading)

Nhiều hàm cùng tên nhưng khác số lượng/kiểu tham số:

```cpp
int tong(int a, int b) {
    return a + b;
}

double tong(double a, double b) {
    return a + b;
}

int tong(int a, int b, int c) {
    return a + b + c;
}
```

## 6. Tham số mặc định (default parameter)

```cpp
void inThongBao(string msg, int soLan = 1) {
    for (int i = 0; i < soLan; i++) cout << msg << endl;
}

int main() {
    inThongBao("Hi");        // in 1 lần (dùng giá trị mặc định)
    inThongBao("Hi", 3);     // in 3 lần
}
```

## 7. Biến cục bộ vs biến toàn cục

```cpp
int diemToanCuc = 100; // biến toàn cục, dùng được ở mọi hàm

void inDiem() {
    int diemCucBo = 50; // chỉ tồn tại trong hàm này
    cout << diemToanCuc << " " << diemCucBo << endl;
}
```

⚠️ Hạn chế dùng biến toàn cục vì dễ gây lỗi khó kiểm soát khi chương trình lớn.

## 8. Ví dụ thực tế: Máy tính đơn giản dùng hàm

```cpp
#include <iostream>
using namespace std;

double tinh(double a, double b, char phepToan) {
    switch (phepToan) {
        case '+': return a + b;
        case '-': return a - b;
        case '*': return a * b;
        case '/':
            if (b == 0) {
                cout << "Loi: chia cho 0" << endl;
                return 0;
            }
            return a / b;
        default:
            cout << "Phep toan khong hop le" << endl;
            return 0;
    }
}

int main() {
    double a, b;
    char pt;
    cin >> a >> pt >> b;
    cout << "Ket qua: " << tinh(a, b, pt) << endl;
    return 0;
}
```

## Bài tập gợi ý

1. Viết hàm `boolean laSoNguyenTo(int n)` trả về `true`/`false`, tái sử dụng cho các bài sau.
2. Viết hàm `int UCLN(int a, int b)` tính ước chung lớn nhất bằng thuật toán Euclid, và hàm `int BCNN(int a, int b)` dùng lại `UCLN`.
3. Viết hàm `void hoanDoi(int &a, int &b)` hoán đổi giá trị 2 biến bằng tham chiếu.
4. Viết hàm đệ quy đơn giản `long long giaiThua(int n)` tính giai thừa (chuẩn bị cho bài đệ quy ở phần Trung cấp).
5. (Nâng cao nhẹ) Viết một bộ hàm nạp chồng `tong()` nhận 2, 3, hoặc 4 số nguyên và trả về tổng tương ứng.
