# Bài 4: Đệ Quy (Recursion)

## 1. Đệ quy là gì?

Đệ quy là khi một hàm **tự gọi lại chính nó** để giải quyết một bài toán nhỏ hơn, cho tới khi chạm tới trường hợp đơn giản nhất (gọi là **điều kiện dừng** — base case) thì không gọi nữa.

**Ví dụ dễ hình dung trước khi vào code:** giống như bạn đứng xếp hàng hỏi "mình đứng thứ mấy?" — bạn hỏi người đứng ngay trước mình "anh đứng thứ mấy?", người đó lại hỏi tiếp người trước họ, cứ thế tới khi hỏi tới người đứng đầu hàng (người này biết chắc mình là "số 1", không cần hỏi ai nữa — đây chính là điều kiện dừng). Sau đó thông tin "dội ngược" lại: người số 1 báo "tôi số 1", người sau đó biết mình là "số 2", cứ thế tới lượt bạn.

## 2. Ví dụ đầu tiên: Tính giai thừa

```cpp
long long giaiThua(int n) {
    if (n == 0) return 1;         // điều kiện dừng (base case)
    return n * giaiThua(n - 1);   // gọi lại chính nó với bài toán nhỏ hơn
}
```

**Giải thích từng bước với `giaiThua(4)`:**
```
giaiThua(4) = 4 * giaiThua(3)
            = 4 * (3 * giaiThua(2))
            = 4 * (3 * (2 * giaiThua(1)))
            = 4 * (3 * (2 * (1 * giaiThua(0))))
            = 4 * (3 * (2 * (1 * 1)))     <- chạm base case, giaiThua(0) = 1
            = 4 * 3 * 2 * 1 * 1 = 24
```

⚠️ **Luôn phải có điều kiện dừng!** Nếu quên, hàm sẽ gọi lại chính nó mãi mãi (gọi là đệ quy vô hạn), khiến chương trình bị crash do tràn bộ nhớ ngăn xếp (stack overflow).

## 3. Ví dụ thứ hai: Dãy Fibonacci

```cpp
int fibonacci(int n) {
    if (n <= 1) return n;                       // base case: fib(0)=0, fib(1)=1
    return fibonacci(n - 1) + fibonacci(n - 2);  // gọi đệ quy 2 lần
}
```

**Giải thích:** để tính `fibonacci(5)`, hàm cần biết `fibonacci(4)` và `fibonacci(3)` trước — mỗi số lại cần 2 số trước nó, tạo thành một "cây" các lời gọi hàm phân nhánh.

> **Lưu ý cho thi HSG:** cách viết Fibonacci đệ quy ở trên tuy dễ hiểu nhưng **rất chậm** với `n` lớn (vì tính đi tính lại rất nhiều lần cùng 1 giá trị). Đây chính là lý do người ta phát minh ra kỹ thuật **Quy hoạch động (Dynamic Programming)** — sẽ học kỹ ở Phần 4, giúp giải bài toán tương tự nhanh hơn rất nhiều.

## 4. Đệ quy trên chuỗi/mảng: Kiểm tra Palindrome

```cpp
bool laDoiXung(string s, int trai, int phai) {
    if (trai >= phai) return true;              // base case: đã duyệt hết hoặc gặp nhau ở giữa
    if (s[trai] != s[phai]) return false;       // phát hiện sai lệch, dừng ngay
    return laDoiXung(s, trai + 1, phai - 1);    // thu nhỏ bài toán: bỏ 2 đầu, xét phần giữa
}
```

## 5. Mỗi lần đệ quy nên trả lời 3 câu hỏi

1. **Điều kiện dừng là gì?** (base case) — trường hợp đơn giản nhất, biết ngay đáp án không cần gọi thêm.
2. **Bài toán lớn được chia nhỏ ra sao?** — làm sao để bài toán tự gọi lại chính nó với input "nhỏ hơn".
3. **Kết hợp kết quả các bài toán nhỏ như thế nào?** — ví dụ giai thừa thì nhân lại, Fibonacci thì cộng lại.

## 6. Ví dụ thực tế: Đếm số bước để hết bánh trong hộp

Bạn có 1 hộp bánh, mỗi ngày ăn một nửa số bánh còn lại (làm tròn xuống), muốn biết bao nhiêu ngày thì hộp bánh hết sạch (0 cái):

```cpp
#include <iostream>
using namespace std;

int demNgay(int soBanh) {
    if (soBanh == 0) return 0;               // hết bánh rồi, dừng, 0 ngày nữa
    return 1 + demNgay(soBanh / 2);          // ăn 1 ngày, còn lại một nửa, đếm tiếp
}

int main() {
    int soBanh;
    cout << "So banh trong hop: ";
    cin >> soBanh;

    cout << "Can " << demNgay(soBanh) << " ngay de an het (lam tron xuong moi ngay)." << endl;
    return 0;
}
```

Với 10 cái bánh: ngày 1 ăn xuống còn 5, ngày 2 còn 2, ngày 3 còn 1, ngày 4 còn 0 → tổng cộng 4 ngày. Hàm `demNgay` tự "lột" từng lớp bài toán ra như bóc từng lớp hành: từ "10 cái" thu nhỏ dần về "0 cái" — đúng tinh thần của đệ quy.

## Bài tập gợi ý

1. Viết hàm đệ quy tính tổng các số từ 1 đến `n`.
2. Viết hàm đệ quy tính lũy thừa `x^n` (x mũ n) với `n` là số nguyên không âm.
3. Viết hàm đệ quy đếm số chữ số của một số nguyên dương `n`.
4. Viết hàm đệ quy tính tổng các chữ số của một số nguyên dương `n` (ví dụ n=123 → 1+2+3=6).
5. (Nâng cao nhẹ) Bài toán "Tháp Hà Nội" (Tower of Hanoi) là bài kinh điển về đệ quy: có 3 cọc, cần chuyển toàn bộ đĩa từ cọc A sang cọc C (dùng cọc B trung gian), mỗi lần chỉ chuyển 1 đĩa và không được đặt đĩa to lên đĩa nhỏ. Tìm hiểu và thử tự viết hàm đệ quy in ra các bước chuyển đĩa.
