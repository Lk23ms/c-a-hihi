# Bài 1: Con Trỏ & Tham Chiếu

## 1. Địa chỉ bộ nhớ là gì?

Mỗi biến khi được khai báo sẽ được cấp một chỗ trong bộ nhớ RAM, và chỗ đó có một "địa chỉ" riêng — giống như mỗi ngôi nhà có một địa chỉ số nhà riêng để người đưa thư tìm tới.

```cpp
int a = 10;
cout << &a << endl; // in ra địa chỉ của biến a, ví dụ: 0x7ffee4b3a9ac
```

Toán tử `&` ở đây (khi đặt trước tên biến) nghĩa là "lấy địa chỉ của".

## 2. Con trỏ (pointer) là gì?

Con trỏ là một biến đặc biệt, **không chứa giá trị thông thường mà chứa địa chỉ của một biến khác**.

```cpp
int a = 10;
int *p = &a;   // p là con trỏ, lưu địa chỉ của a

cout << p << endl;   // in ra địa chỉ của a
cout << *p << endl;  // in ra 10 -> *p nghĩa là "giá trị tại địa chỉ mà p đang trỏ tới"
```

**Giải thích:** `int *p` khai báo `p` là một con trỏ kiểu `int`. Toán tử `*` có 2 vai trò khác nhau tùy ngữ cảnh:
- Khi khai báo (`int *p`): nghĩa là "p là con trỏ".
- Khi dùng sau đó (`*p`): nghĩa là "giá trị nằm ở địa chỉ mà p trỏ tới" — gọi là **dereference** (giải tham chiếu).

**Ví dụ dễ hình dung:** hãy tưởng tượng biến `a` là căn nhà thật, chứa đồ đạc (giá trị 10) bên trong. Con trỏ `p` giống như một **mảnh giấy ghi địa chỉ nhà** của `a` (không phải căn nhà thật, chỉ là tờ giấy ghi địa chỉ). Khi bạn cầm tờ giấy đó và tới đúng địa chỉ ghi trên đó, mở cửa vào nhà, bạn sẽ thấy đồ đạc thật bên trong — đó chính là ý nghĩa của `*p`.

## 3. Thay đổi giá trị qua con trỏ

```cpp
int a = 10;
int *p = &a;

*p = 20;  // thay đổi giá trị tại địa chỉ p đang trỏ tới, tức là thay đổi a
cout << a << endl; // 20
```

Vì `p` trỏ thẳng tới nhà của `a`, nên sửa đồ đạc "qua tờ giấy địa chỉ" cũng chính là sửa đồ đạc thật trong nhà `a`.

## 4. Con trỏ và tham chiếu (`&`) trong hàm — ôn lại và mở rộng

Ở phần Cơ bản, ta đã học truyền tham chiếu bằng `&` trong hàm. Con trỏ cũng làm được việc tương tự, nhưng viết tường minh hơn:

```cpp
// Cách dùng tham chiếu (đã học ở phần Cơ bản)
void tangThamChieu(int &x) {
    x++;
}

// Cách dùng con trỏ - làm việc tương tự nhưng phải "giải tham chiếu"
void tangConTro(int *x) {
    (*x)++;
}

int main() {
    int a = 5;
    tangThamChieu(a);   // gọi bình thường
    cout << a << endl;  // 6

    int b = 5;
    tangConTro(&b);      // phải truyền địa chỉ &b
    cout << b << endl;   // 6
}
```

> **So sánh nhanh:** tham chiếu (`&`) dễ dùng hơn và là lựa chọn ưu tiên trong C++ hiện đại. Con trỏ mạnh mẽ và linh hoạt hơn (có thể trỏ tới `nullptr`, có thể thay đổi trỏ sang biến khác giữa chừng), nhưng dễ gây lỗi hơn nếu dùng sai. Con trỏ đặc biệt quan trọng khi học tới cấp phát động ở bài tiếp theo.

## 5. Con trỏ null (không trỏ vào đâu cả)

```cpp
int *p = nullptr; // con trỏ chưa trỏ tới biến nào

if (p == nullptr) {
    cout << "Con tro dang rong" << endl;
}
```

⚠️ Nếu cố tình `*p` khi `p` đang là `nullptr`, chương trình sẽ bị lỗi crash ngay lập tức (giống việc mở cửa vào một địa chỉ nhà không tồn tại).

## 6. Ví dụ thực tế: Đổi chỗ ngồi 2 học sinh

Giáo viên muốn viết một hàm hoán đổi chỗ ngồi (số thứ tự bàn) của 2 học sinh bất kỳ trong lớp:

```cpp
#include <iostream>
using namespace std;

void doiCho(int *choA, int *choB) {
    int tam = *choA;
    *choA = *choB;
    *choB = tam;
}

int main() {
    int hocSinh1 = 5;  // đang ngồi bàn số 5
    int hocSinh2 = 12; // đang ngồi bàn số 12

    doiCho(&hocSinh1, &hocSinh2);

    cout << "Hoc sinh 1 gio ngoi ban: " << hocSinh1 << endl; // 12
    cout << "Hoc sinh 2 gio ngoi ban: " << hocSinh2 << endl; // 5

    return 0;
}
```

Nhờ dùng con trỏ (`*choA`, `*choB`), hàm `doiCho` sửa được trực tiếp giá trị gốc của `hocSinh1` và `hocSinh2` ở ngoài `main()` — nếu dùng truyền giá trị thông thường (không có `*`), hàm chỉ đổi chỗ trên "bản sao" và không ảnh hưởng gì tới 2 biến gốc cả.

## Bài tập gợi ý

1. Viết chương trình khai báo 1 biến `int`, in ra cả giá trị và địa chỉ của nó.
2. Viết hàm `void nhanDoi(int *x)` nhân đôi giá trị của biến được trỏ tới.
3. Cho 3 con trỏ trỏ tới 3 biến số nguyên khác nhau, viết chương trình sắp xếp 3 giá trị đó tăng dần (thay đổi trực tiếp qua con trỏ).
4. Giải thích sự khác nhau giữa `int *p = &a;` và `int b = a;` — cái nào tạo ra "bản sao", cái nào chỉ là "địa chỉ tham chiếu tới bản gốc"?
5. (Nâng cao nhẹ) Con trỏ có thể "trỏ tới con trỏ khác" không (`int **pp`)? Thử tìm hiểu và viết một ví dụ đơn giản minh họa khái niệm con trỏ đôi (pointer to pointer).
