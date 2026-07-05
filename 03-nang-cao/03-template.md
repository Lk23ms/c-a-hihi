# Bài 3: Template

## 1. Vấn đề: viết hàm dùng chung cho nhiều kiểu dữ liệu

Giả sử cần viết hàm tìm số lớn nhất trong 2 số. Nếu không có template, phải viết lặp lại cho từng kiểu:

```cpp
int max_int(int a, int b) {
    return (a > b) ? a : b;
}

double max_double(double a, double b) {
    return (a > b) ? a : b;
}
```

Hai hàm này logic **giống hệt nhau**, chỉ khác kiểu dữ liệu — rất lãng phí công sức viết lại. Template giải quyết đúng vấn đề này.

## 2. Hàm template

```cpp
template <typename T>
T timMax(T a, T b) {
    return (a > b) ? a : b;
}

int main() {
    cout << timMax(3, 7) << endl;         // T tự động là int -> 7
    cout << timMax(3.5, 2.1) << endl;     // T tự động là double -> 3.5
    cout << timMax(string("abc"), string("xyz")) << endl; // T là string -> "xyz"
}
```

**Giải thích:** `template <typename T>` nghĩa là "T là một kiểu dữ liệu chưa xác định, sẽ được quyết định sau". Khi gọi `timMax(3, 7)`, trình biên dịch tự nhận ra `T` nên là `int` (vì 2 tham số truyền vào đều là số nguyên), và tự tạo ra 1 phiên bản hàm `timMax` dành riêng cho `int` phía sau hậu trường — bạn không cần viết tay phiên bản đó.

**Ví dụ dễ hình dung:** template giống như một **cái khuôn làm bánh đa năng**, có thể đổ bất kỳ loại bột nào vào (bột mì, bột gạo, bột năng — tương ứng với `int`, `double`, `string`), miễn là quy trình đổ khuôn giống nhau. Bạn không cần mua riêng 1 cái khuôn cho mỗi loại bột, chỉ cần 1 khuôn "tổng quát" dùng chung được cho mọi loại.

## 3. Class template

Không chỉ hàm, class cũng có thể dùng template:

```cpp
template <typename T>
class Hop {
private:
    T giaTri;

public:
    Hop(T gt) { giaTri = gt; }

    T layGiaTri() {
        return giaTri;
    }

    void datGiaTri(T gt) {
        giaTri = gt;
    }
};

int main() {
    Hop<int> hopSo(42);
    cout << hopSo.layGiaTri() << endl; // 42

    Hop<string> hopChu("Xin chao");
    cout << hopChu.layGiaTri() << endl; // "Xin chao"
}
```

**Giải thích:** `Hop<int>` và `Hop<string>` thực chất là 2 "phiên bản" khác nhau của cùng 1 class `Hop`, được sinh ra tự động tùy vào kiểu dữ liệu bạn chỉ định trong dấu `< >`.

## 4. Template thực ra đã được dùng từ lâu — chính là STL!

Những gì bạn đã học ở bài STL cơ bản (`vector<int>`, `pair<string, int>`) chính là các **class template** có sẵn của C++:

```cpp
vector<int> v;       // vector là template, <int> chỉ định kiểu phần tử
vector<string> vs;   // cùng 1 class vector, nhưng dùng cho kiểu string
pair<int, double> p; // pair cũng là template, nhận 2 kiểu khác nhau
```

Giờ bạn đã hiểu vì sao cú pháp `vector<...>` lại có dấu `< >` — đó chính là cách chỉ định kiểu `T` cho template, y hệt như `Hop<int>` ở ví dụ trên.

## 5. Ví dụ thực tế: Ngăn kéo đựng đồ dùng chung cho nhiều loại vật dụng

```cpp
#include <iostream>
using namespace std;

template <typename T>
class NganKeo {
private:
    T vatDung;
    bool coDoTrongNganKeo;

public:
    NganKeo() {
        coDoTrongNganKeo = false;
    }

    void cat(T do_) {
        vatDung = do_;
        coDoTrongNganKeo = true;
        cout << "Da cat do vao ngan keo." << endl;
    }

    T lay() {
        coDoTrongNganKeo = false;
        return vatDung;
    }

    bool kiemTraCoDo() {
        return coDoTrongNganKeo;
    }
};

int main() {
    NganKeo<string> nganKeoTaiLieu;
    nganKeoTaiLieu.cat("Giay khai sinh");
    cout << "Lay ra: " << nganKeoTaiLieu.lay() << endl;

    NganKeo<int> nganKeoTien;
    nganKeoTien.cat(500000);
    cout << "Lay ra: " << nganKeoTien.lay() << " dong" << endl;

    return 0;
}
```

Cùng 1 "thiết kế ngăn kéo" (`NganKeo`) nhưng dùng được cho cả tài liệu (kiểu `string`) lẫn tiền (kiểu `int`) — không cần thiết kế riêng "ngăn kéo đựng tài liệu" và "ngăn kéo đựng tiền" như 2 class khác nhau.

## Bài tập gợi ý

1. Viết hàm template `T tinhTong(T a, T b, T c)` tính tổng 3 giá trị cùng kiểu (dùng được với cả `int` và `double`).
2. Viết hàm template `void hoanDoi(T &a, T &b)` hoán đổi 2 giá trị bất kỳ kiểu gì (kết hợp kiến thức tham chiếu đã học).
3. Viết class template `CapDoi<T>` tương tự `pair`, lưu 2 giá trị cùng kiểu `T`, có phương thức trả về giá trị lớn hơn trong 2 giá trị đó.
4. Viết hàm template tìm giá trị lớn nhất trong 1 mảng bất kỳ kiểu gì (`T timMaxMang(T a[], int n)`), thử với cả mảng `int` và mảng `double`.
5. (Nâng cao nhẹ) Tìm hiểu template với 2 kiểu tham số khác nhau, ví dụ `template <typename K, typename V> class TuDien { K khoa; V giaTri; };` — thử áp dụng để mô phỏng một "từ điển" đơn giản gồm 1 cặp khóa-giá trị.
