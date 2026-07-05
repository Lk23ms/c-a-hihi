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

## 2. Bit và Byte là gì?

Trước khi vào bảng kiểu dữ liệu, cần hiểu 2 khái niệm nền tảng này — vì chúng quyết định máy tính lưu trữ và tính toán mọi thứ như thế nào.

**Bit** là đơn vị nhỏ nhất trong máy tính, chỉ có thể mang 1 trong 2 giá trị: `0` hoặc `1` (giống công tắc điện: tắt hoặc bật). Máy tính không hiểu chữ, hiểu số, hay hiểu hình ảnh như con người — nó chỉ hiểu chuỗi các bit 0 và 1 mà thôi.

**Byte** = 8 bit ghép lại với nhau. Đây là đơn vị người ta hay dùng để đo "sức chứa" của dữ liệu, vì 1 bit lẻ quá nhỏ để làm gì hữu ích.

Ví dụ dễ hình dung: 8 bit (1 byte) có thể tạo ra `2^8 = 256` cách kết hợp khác nhau (từ `00000000` đến `11111111`). Đó là lý do kiểu `char` (1 byte) chỉ lưu được 256 giá trị khác nhau — vừa đủ để biểu diễn các ký tự cơ bản như chữ cái, số, và ký hiệu trong bảng mã ASCII.

Các đơn vị lớn hơn mà bạn hay thấy khi mua USB, thẻ nhớ, ổ cứng:

| Đơn vị | Bằng bao nhiêu | Ví dụ thực tế |
|---|---|---|
| 1 Byte | 8 bit | 1 ký tự, ví dụ chữ `'A'` |
| 1 KB (Kilobyte) | ~1,000 byte | 1 trang văn bản ngắn |
| 1 MB (Megabyte) | ~1,000 KB | 1 bài hát MP3 |
| 1 GB (Gigabyte) | ~1,000 MB | 1 bộ phim HD |

**Vì sao chuyện này liên quan đến việc khai báo biến?** Vì mỗi kiểu dữ liệu trong C++ chiếm một số byte cố định trong bộ nhớ máy tính, và số byte đó quyết định nó lưu được giá trị lớn tới đâu. Ví dụ `int` chiếm 4 byte = 32 bit, nên nó lưu được khoảng `2^32` giá trị khác nhau (trải từ số âm đến số dương) — đó là lý do `int` có giới hạn khoảng ±2.1 tỷ như bảng dưới đây sẽ cho thấy.

## 3. Các kiểu dữ liệu cơ bản

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

## 4. Khai báo và gán giá trị biến

```cpp
int tuoi = 20;
double diemTB = 8.5;
char kyTu = 'A';
bool daDangKy = true;
string ten = "Khoi";

// Khai báo nhiều biến cùng kiểu
int a = 1, b = 2, c = 3;
```

**Giải thích:** mỗi dòng là một "cái hộp" được đặt tên và gán sẵn giá trị bên trong — hộp `tuoi` chứa số 20, hộp `ten` chứa chữ "Khoi". Khi cần dùng lại giá trị đó, chỉ cần gọi tên hộp (biến) ra là được, không cần gõ lại giá trị.

**Ví dụ dễ hình dung:** biến giống như các ngăn tủ trong nhà, mỗi ngăn có nhãn dán tên riêng. Ngăn `tuoi` bạn dán nhãn "Tuổi" và bỏ vào tờ giấy ghi số 20. Ngăn `ten` dán nhãn "Tên" và bỏ giấy ghi "Khoi". Khi cần biết tuổi, bạn chỉ việc mở đúng ngăn có nhãn "Tuổi" ra xem, không cần nhớ nó nằm ở đâu trong cả căn phòng.

## 5. Nhập / xuất dữ liệu với cin, cout

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

## 6. Ép kiểu (type casting)

```cpp
int a = 7, b = 2;
double ketQua = (double)a / b;   // ép kiểu tường minh -> 3.5
double sai = a / b;              // 3 (vì int / int = int, mất phần thập phân)
```

**Giải thích:** dòng `(double)a` là "ép" biến `a` (vốn là `int`) tạm thời biến thành kiểu `double` trước khi chia. Nhờ vậy phép chia `7.0 / 2` sẽ ra `3.5` đầy đủ, thay vì `int / int` chỉ giữ lại phần nguyên và làm mất phần thập phân (`7 / 2` cho ra `3`, phần `.5` bị "cắt cụt" mất luôn chứ không làm tròn).

**Ví dụ dễ hình dung:** giống như bạn có 7 cái bánh, chia đều cho 2 người. Nếu tính kiểu "int" (chỉ đếm bánh nguyên), mỗi người được 3 cái, còn dư nửa cái bị vứt bỏ không tính. Nhưng nếu tính kiểu "double" (cho phép chia nhỏ bánh ra), mỗi người sẽ được đúng 3.5 cái — công bằng và chính xác hơn.

## 7. Ví dụ thực tế: Tính tiền mua trà sữa

Giả sử bạn đi mua trà sữa, cần biết phải trả bao nhiêu tiền và người bán thối lại bao nhiêu:

```cpp
#include <iostream>
using namespace std;

int main() {
    int giaTien;      // giá 1 ly trà sữa, vd 35000 đồng
    int soLuong;      // mua mấy ly
    int tienDua;      // số tiền mình đưa cho người bán

    cout << "Gia 1 ly tra sua: ";
    cin >> giaTien;
    cout << "So luong ly: ";
    cin >> soLuong;
    cout << "So tien dua: ";
    cin >> tienDua;

    int tongTien = giaTien * soLuong;
    int tienThoi = tienDua - tongTien;

    cout << "Tong tien phai tra: " << tongTien << " dong" << endl;
    cout << "Tien duoc thoi lai: " << tienThoi << " dong" << endl;

    return 0;
}
```

Chạy thử: giá 35000, mua 3 ly, đưa 150000 → tổng tiền = 105000, được thối lại 45000. Y hệt cách máy tính tiền ở quán trà sữa hoạt động bên trong đó!

## Ví dụ thực tế 2: Tính điểm trung bình 3 môn

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
