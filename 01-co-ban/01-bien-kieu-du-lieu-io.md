# Bài 1: Biến, Kiểu Dữ Liệu, I/O Cơ Bản

## 1. Cấu trúc chương trình C++ tối giản

```cpp
#include <iostream>
using namespace std;

int main() {
    cout << "Hello, C++!" << endl;
    return 0;
}
```

- `#include <iostream>`: nạp thư viện nhập/xuất chuẩn.
- `main()`: hàm bắt buộc, chương trình luôn bắt đầu chạy từ đây.
- `cout`: xuất dữ liệu ra màn hình. `cin`: nhập dữ liệu từ bàn phím.
- `return 0;`: báo chương trình kết thúc thành công.

## 2. Các kiểu dữ liệu cơ bản

| Kiểu | Ý nghĩa | Kích thước (thường gặp) | Ví dụ giá trị |
|---|---|---|---|
| `int` | số nguyên | 4 byte | `-5`, `100` |
| `long long` | số nguyên lớn | 8 byte | `10000000000LL` |
| `float` | số thực độ chính xác đơn | 4 byte | `3.14f` |
| `double` | số thực độ chính xác kép | 8 byte | `3.14159` |
| `char` | 1 ký tự | 1 byte | `'a'` |
| `bool` | đúng/sai | 1 byte | `true`, `false` |
| `string` | chuỗi ký tự | thay đổi | `"xin chao"` |

> **Lưu ý cho thi HSG:** `int` chỉ chứa được khoảng ±2.1 tỷ. Nếu bài toán có kết quả lớn hơn, luôn dùng `long long` để tránh tràn số (overflow) — lỗi rất hay gặp khi thi.

## 3. Khai báo và gán giá trị biến

```cpp
int tuoi = 20;
double diemTB = 8.5;
char kyTu = 'A';
bool daDangKy = true;
string ten = "Khoi";

// Khai báo nhiều biến cùng kiểu
int a = 1, b = 2, c = 3;
```

## 4. Nhập / xuất dữ liệu với cin, cout

```cpp
#include <iostream>
using namespace std;

int main() {
    int tuoi;
    string ten;

    cout << "Nhap ten: ";
    cin >> ten;              // đọc 1 từ (dừng ở khoảng trắng)

    cout << "Nhap tuoi: ";
    cin >> tuoi;

    cout << "Xin chao " << ten << ", ban " << tuoi << " tuoi." << endl;
    return 0;
}
```

### Đọc cả một dòng (có khoảng trắng)
```cpp
string hoTen;
cin.ignore(); // bỏ ký tự newline còn sót lại trong buffer
getline(cin, hoTen);
```

## 5. Ép kiểu (type casting)

```cpp
int a = 7, b = 2;
double ketQua = (double)a / b;   // ép kiểu tường minh -> 3.5
double sai = a / b;              // 3 (vì int / int = int, mất phần thập phân)
```

## 6. Ví dụ thực tế: Tính điểm trung bình 3 môn

```cpp
#include <iostream>
using namespace std;

int main() {
    double toan, van, anh;
    cout << "Nhap diem Toan, Van, Anh: ";
    cin >> toan >> van >> anh;

    double trungBinh = (toan + van + anh) / 3.0;
    cout << "Diem trung binh: " << trungBinh << endl;

    if (trungBinh >= 8.0)
        cout << "Xep loai: Gioi" << endl;

    return 0;
}
```

## Bài tập gợi ý

1. Viết chương trình nhập vào bán kính `r` của hình tròn (kiểu `double`), tính và in ra chu vi và diện tích (dùng hằng số PI = 3.14159).
2. Nhập vào 2 số nguyên `a`, `b`. In ra tổng, hiệu, tích, và thương (dạng số thực) của chúng.
3. Nhập họ tên đầy đủ của một người (có khoảng trắng) bằng `getline`, in ra câu chào có chứa tên đó.
4. Cho biết sự khác nhau giữa kết quả của `7 / 2` và `7.0 / 2` trong C++. Viết chương trình kiểm chứng.
5. (Nâng cao nhẹ) Một số `int` có thể lưu tối đa giá trị nào? Viết chương trình gán giá trị vượt quá giới hạn đó cho biến `int` và quan sát hiện tượng "tràn số" (overflow) xảy ra.
