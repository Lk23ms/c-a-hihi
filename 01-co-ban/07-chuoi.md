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

## 9. Ví dụ thực tế: Kiểm tra chuỗi đối xứng (Palindrome)

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
