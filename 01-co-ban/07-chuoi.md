# Bài 7: Chuỗi Ký Tự (String)

## 1. Khai báo chuỗi với kiểu `string`

```cpp
#include <string>
using namespace std;

string s = "Xin chao";
string ten;
cin >> ten;          // đọc 1 từ (dừng ở khoảng trắng)
getline(cin, ten);   // đọc cả dòng (bao gồm khoảng trắng)
```

## 2. Các thao tác cơ bản

```cpp
string s = "Hello";

cout << s.length() << endl;  // 5 (độ dài chuỗi), có thể dùng s.size()

cout << s[0] << endl;        // 'H' - truy cập ký tự như mảng

s += " World";               // nối chuỗi -> "Hello World"
string s2 = s + "!";         // ghép 2 chuỗi

s[0] = 'h';                  // sửa 1 ký tự -> "hello World"
```

## 3. So sánh chuỗi

```cpp
string a = "apple", b = "banana";

if (a == b) { ... }   // so sánh bằng
if (a < b) { ... }    // so sánh theo thứ tự từ điển (alphabet), "apple" < "banana"
```

## 4. Cắt chuỗi con: `substr`

```cpp
string s = "Hello World";
string con = s.substr(0, 5);   // "Hello" (bắt đầu từ vị trí 0, lấy 5 ký tự)
string con2 = s.substr(6);     // "World" (từ vị trí 6 đến hết)
```

## 5. Tìm kiếm trong chuỗi: `find`

```cpp
string s = "Hello World";
int viTri = s.find("World"); // trả về vị trí bắt đầu, ở đây là 6
if (s.find("xyz") == string::npos) {
    cout << "Khong tim thay" << endl;
}
```

## 6. Duyệt từng ký tự trong chuỗi

```cpp
string s = "Hello";
for (int i = 0; i < s.length(); i++) {
    cout << s[i] << " ";
}

// hoặc dùng range-based for (C++11 trở lên)
for (char c : s) {
    cout << c << " ";
}
```

## 7. Chuyển đổi kiểu chữ (upper/lower)

```cpp
#include <cctype>

char c = 'a';
cout << (char)toupper(c) << endl; // 'A'
cout << (char)tolower('B') << endl; // 'b'

// Chuyển cả chuỗi
string s = "Hello";
for (int i = 0; i < s.length(); i++) {
    s[i] = toupper(s[i]);
}
// s = "HELLO"
```

## 8. Chuyển đổi giữa string và số

```cpp
#include <string>

string s = "123";
int n = stoi(s);        // string to int -> 123
double d = stod("3.14"); // string to double -> 3.14

int x = 456;
string s2 = to_string(x); // int to string -> "456"
```

## 9. Ví dụ thực tế: Kiểm tra mật khẩu có đủ mạnh không

Khi đăng ký tài khoản, nhiều web yêu cầu mật khẩu phải đủ dài và có cả chữ lẫn số. Đây là cách kiểm tra đơn giản:

```cpp
#include <iostream>
#include <string>
#include <cctype>
using namespace std;

int main() {
    string matKhau;
    cout << "Nhap mat khau: ";
    cin >> matKhau;

    bool coChu = false, coSo = false;

    for (int i = 0; i < matKhau.length(); i++) {
        if (isalpha(matKhau[i])) coChu = true; // isalpha: kiểm tra có phải chữ cái
        if (isdigit(matKhau[i])) coSo = true;  // isdigit: kiểm tra có phải chữ số
    }

    if (matKhau.length() >= 8 && coChu && coSo) {
        cout << "Mat khau du manh!" << endl;
    } else {
        cout << "Mat khau qua yeu. Can it nhat 8 ky tu, co ca chu va so." << endl;
    }

    return 0;
}
```

Thử với `"abc123"` (6 ký tự) → yếu vì chưa đủ 8 ký tự. Thử với `"khoi2010"` (8 ký tự, có chữ lẫn số) → đủ mạnh. Đây chính xác là logic đơn giản đứng sau ô "mật khẩu yếu/mạnh" mà bạn hay thấy khi đăng ký tài khoản trên các trang web.

## Ví dụ thực tế 2: Kiểm tra chuỗi đối xứng (Palindrome)

```cpp
#include <iostream>
#include <string>
using namespace std;

int main() {
    string s;
    getline(cin, s);

    int trai = 0, phai = s.length() - 1;
    bool doiXung = true;

    while (trai < phai) {
        if (s[trai] != s[phai]) {
            doiXung = false;
            break;
        }
        trai++;
        phai--;
    }

    cout << (doiXung ? "La chuoi doi xung" : "Khong doi xung") << endl;
    return 0;
}
```

## Bài tập gợi ý

1. Đếm số lượng nguyên âm (a, e, i, o, u) xuất hiện trong một chuỗi (không phân biệt hoa/thường).
2. Viết chương trình đảo ngược một chuỗi (không dùng hàm có sẵn `reverse`).
3. Đếm số từ trong một câu (giả sử các từ cách nhau đúng 1 khoảng trắng).
4. Viết chương trình kiểm tra 2 chuỗi có phải là "anagram" của nhau không (chứa cùng tập ký tự với cùng số lượng, ví dụ "listen" và "silent").
5. (Nâng cao nhẹ) Cho một chuỗi chỉ gồm chữ cái, tìm ký tự xuất hiện nhiều nhất và số lần xuất hiện của nó. Đây là bước đệm quan trọng để giải các bài nén dữ liệu (Huffman coding) sau này.
