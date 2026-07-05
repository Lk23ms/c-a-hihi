# Bài 2: Toán Tử & Biểu Thức

## 1. Toán tử số học

| Toán tử | Ý nghĩa | Ví dụ |
|---|---|---|
| `+` | cộng | `5 + 3 = 8` |
| `-` | trừ | `5 - 3 = 2` |
| `*` | nhân | `5 * 3 = 15` |
| `/` | chia | `5 / 3 = 1` (chia nguyên nếu cả 2 là int) |
| `%` | chia lấy dư | `5 % 3 = 2` |

```cpp
int a = 10, b = 3;
cout << a / b << endl;  // 3
cout << a % b << endl;  // 1
```

> **Mẹo thi HSG:** `%` cực kỳ hữu ích để kiểm tra chẵn/lẻ (`n % 2 == 0`), tách chữ số của một số (`n % 10` lấy chữ số cuối), hoặc xử lý bài toán chu kỳ (modulo).

## 2. Toán tử gán và gán rút gọn

```cpp
int x = 5;
x += 3;  // x = x + 3 -> 8
x -= 2;  // x = x - 2 -> 6
x *= 2;  // x = x * 2 -> 12
x /= 4;  // x = x / 4 -> 3
x %= 2;  // x = x % 2 -> 1
```

## 3. Toán tử tăng/giảm

```cpp
int i = 5;
i++;   // tăng sau: dùng giá trị i rồi mới +1
++i;   // tăng trước: +1 trước rồi mới dùng giá trị

int j = 5;
cout << j++ << endl; // in ra 5, sau đó j = 6
cout << ++j << endl; // j = 7 trước, rồi in ra 7
```

**Giải thích:** đây là chỗ dễ gây rối nhất cho người mới học. Cả `i++` và `++i` cuối cùng đều làm `i` tăng thêm 1, khác nhau ở chỗ: **thời điểm** giá trị đó được "dùng" (ví dụ để in ra hay gán cho biến khác).

**Ví dụ dễ hình dung:** tưởng tượng bạn đang xếp hàng mua vé, số thứ tự hiện tại là 5.
- `j++` (tăng sau) giống như: nhân viên gọi "Số 5!" (dùng giá trị cũ trước), rồi mới cầm bảng lật sang số 6 cho lượt sau.
- `++j` (tăng trước) giống như: nhân viên lật bảng số sang 7 trước, rồi mới gọi "Số 7!" (dùng giá trị mới luôn).

Trong phần lớn trường hợp viết vòng lặp đơn giản (`for (int i = 0; i < n; i++)`), dùng `i++` hay `++i` đều cho kết quả giống nhau — sự khác biệt chỉ thật sự quan trọng khi bạn dùng chung trong một biểu thức lớn hơn như ví dụ `cout << j++` ở trên.

## 4. Toán tử so sánh

| Toán tử | Ý nghĩa |
|---|---|
| `==` | bằng |
| `!=` | khác |
| `>` `<` | lớn hơn / nhỏ hơn |
| `>=` `<=` | lớn hơn hoặc bằng / nhỏ hơn hoặc bằng |

⚠️ Lỗi phổ biến: nhầm `=` (gán) với `==` (so sánh).
```cpp
if (a = 5) { ... }   // SAI: đây là gán, luôn đúng vì a=5 khác 0
if (a == 5) { ... }  // ĐÚNG: so sánh a có bằng 5 không
```

## 5. Toán tử logic

| Toán tử | Ý nghĩa |
|---|---|
| `&&` | VÀ (AND) |
| `\|\|` | HOẶC (OR) |
| `!` | PHỦ ĐỊNH (NOT) |

```cpp
int tuoi = 20;
bool coBangLai = true;

if (tuoi >= 18 && coBangLai) {
    cout << "Duoc phep lai xe" << endl;
}
```

## 6. Độ ưu tiên toán tử (quan trọng)

Thứ tự ưu tiên giảm dần (rút gọn, các trường hợp hay dùng):
1. `()` — ngoặc đơn
2. `!`, `++`, `--` (đơn hạng)
3. `*`, `/`, `%`
4. `+`, `-`
5. `<`, `<=`, `>`, `>=`
6. `==`, `!=`
7. `&&`
8. `||`
9. `=`, `+=`, `-=`, ...

```cpp
int ketQua = 2 + 3 * 4;      // 14, không phải 20
int ketQua2 = (2 + 3) * 4;   // 20, vì ngoặc ưu tiên trước
```

**Giải thích:** máy tính luôn làm phép nhân/chia trước, cộng/trừ sau — giống hệt quy tắc toán học bạn học ở trường (nhân chia trước, cộng trừ sau). Muốn bắt máy tính cộng trước, phải "ra lệnh" bằng cách bỏ vào ngoặc `()`.

**Ví dụ dễ hình dung:** giống như khi mẹ nhờ bạn "mua 2 cái bánh, cộng thêm 3 gói kẹo mỗi gói 4 cái" — bạn tự hiểu phải nhân trước (`3 gói x 4 cái = 12 cái kẹo`) rồi mới cộng với 2 cái bánh, ra tổng 14 món. Còn nếu mẹ nói rõ "(2 cái bánh cộng 3 gói kẹo) rồi nhân với 4" thì bạn phải cộng trước, được 5, rồi mới nhân 4 ra 20 — hai cách tính cho ra kết quả hoàn toàn khác nhau, y hệt như ví dụ code ở trên.

## 7. Ví dụ thực tế: Chia kẹo cho các bạn trong lớp

Bạn có một túi kẹo, muốn chia đều cho các bạn trong lớp, dư ra mấy viên thì mình ăn:

```cpp
#include <iostream>
using namespace std;

int main() {
    int soKeo, soBan;
    cout << "So keo: ";
    cin >> soKeo;
    cout << "So ban trong lop: ";
    cin >> soBan;

    int moiNguoiDuoc = soKeo / soBan;  // chia nguyên: mỗi bạn được bấy nhiêu viên
    int duLai = soKeo % soBan;         // chia lấy dư: số kẹo còn thừa

    cout << "Moi ban duoc: " << moiNguoiDuoc << " vien keo" << endl;
    cout << "Du lai: " << duLai << " vien (phan nay ban tu an nhe)" << endl;

    return 0;
}
```

Ví dụ: có 23 viên kẹo, chia cho 5 bạn → mỗi bạn được 4 viên (`23 / 5 = 4`), dư lại 3 viên (`23 % 5 = 3`). Đây chính là lý do phép `%` (chia lấy dư) hữu ích: nó giúp mình biết chính xác phần "lẻ" còn sót lại sau khi chia đều.

## Ví dụ thực tế 2: Kiểm tra năm nhuận

```cpp
#include <iostream>
using namespace std;

int main() {
    int nam;
    cout << "Nhap nam: ";
    cin >> nam;

    bool nhuan = (nam % 4 == 0 && nam % 100 != 0) || (nam % 400 == 0);

    if (nhuan)
        cout << nam << " la nam nhuan" << endl;
    else
        cout << nam << " khong phai nam nhuan" << endl;

    return 0;
}
```

## Bài tập gợi ý

1. Nhập một số nguyên `n` có 3 chữ số, tách và in ra từng chữ số (hàng trăm, hàng chục, hàng đơn vị) bằng `/` và `%`.
2. Viết biểu thức logic kiểm tra xem một điểm số `x` có nằm trong khoảng `[0, 10]` hay không.
3. Cho 3 cạnh `a, b, c`. Viết biểu thức logic kiểm tra chúng có thể tạo thành một tam giác hợp lệ không (tổng 2 cạnh bất kỳ luôn lớn hơn cạnh còn lại).
4. Giải thích tại sao `5 / 2 * 2` cho kết quả `4` chứ không phải `5` trong C++ với các biến kiểu `int`.
5. (Nâng cao nhẹ) Viết chương trình hoán đổi giá trị 2 biến `a`, `b` mà **không dùng biến tạm**, chỉ dùng phép cộng/trừ hoặc XOR (`^`).
