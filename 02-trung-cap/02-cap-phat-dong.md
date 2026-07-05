# Bài 2: Cấp Phát Động (Dynamic Memory)

## 1. Vấn đề với mảng tĩnh

Ở phần Cơ bản, khi khai báo mảng ta phải biết trước kích thước:
```cpp
int a[100]; // phải đoán trước tối đa 100 phần tử
```

Nhưng nếu không biết trước cần bao nhiêu phần tử (ví dụ số lượng phụ thuộc vào input người dùng nhập), khai báo mảng tĩnh sẽ lãng phí bộ nhớ (khai báo dư quá nhiều) hoặc bị thiếu (khai báo không đủ).

## 2. Cấp phát động với `new` và `delete`

```cpp
int n;
cin >> n;

int *a = new int[n]; // cấp phát đúng n phần tử, tùy theo lúc chạy chương trình

for (int i = 0; i < n; i++) {
    cin >> a[i];
}

delete[] a; // giải phóng bộ nhớ sau khi dùng xong
```

**Giải thích:** `new int[n]` là lời "yêu cầu" hệ điều hành cấp cho mình một vùng nhớ đủ chứa `n` số nguyên, và trả về con trỏ `a` trỏ tới đầu vùng nhớ đó. Sau khi dùng xong, phải gọi `delete[] a;` để "trả lại" vùng nhớ đó cho hệ thống — nếu quên, sẽ gây ra hiện tượng **rò rỉ bộ nhớ (memory leak)**.

**Ví dụ dễ hình dung:** giống như thuê phòng khách sạn theo đúng số người trong đoàn (thay vì luôn đặt sẵn phòng 100 người dù đoàn chỉ có 5 người). `new` giống như hành động "đặt phòng" — bạn nói cho lễ tân biết cần bao nhiêu giường, họ cấp đúng phòng đó cho bạn. `delete[]` giống như "trả phòng" khi checkout — nếu bạn quên trả phòng, khách sạn (bộ nhớ máy tính) sẽ dần hết phòng trống cho người khác thuê.

## 3. Cấp phát 1 biến đơn (không phải mảng)

```cpp
int *p = new int;   // cấp phát 1 ô nhớ int
*p = 42;
cout << *p << endl; // 42
delete p;            // giải phóng (lưu ý: không có [] vì không phải mảng)
```

⚠️ Nhớ đúng cặp: `new` đi với `delete`, `new[]` đi với `delete[]`. Dùng sai cặp có thể gây lỗi khó phát hiện.

## 4. Con trỏ "treo" (dangling pointer) — lỗi thường gặp

```cpp
int *p = new int(10);
delete p;

cout << *p << endl; // NGUY HIỂM: p đã bị giải phóng, đây là "con trỏ treo"
```

Sau khi `delete`, vùng nhớ đó không còn thuộc về bạn nữa (có thể bị cấp cho chương trình khác dùng), nên truy cập lại `*p` là hành vi không an toàn — chương trình có thể chạy sai, hoặc crash.

**Cách phòng tránh đơn giản:** sau khi `delete p;`, luôn gán ngay `p = nullptr;` để nếu lỡ dùng lại, chương trình sẽ báo lỗi rõ ràng ngay thay vì âm thầm chạy sai.

## 5. Ví dụ thực tế: Quản lý danh sách khách mời tiệc sinh nhật

Bạn tổ chức tiệc sinh nhật, số khách mời chỉ biết được lúc chạy chương trình (do người dùng nhập vào), nên cần cấp phát động:

```cpp
#include <iostream>
using namespace std;

int main() {
    int soKhach;
    cout << "Ban moi bao nhieu khach: ";
    cin >> soKhach;

    string *danhSachKhach = new string[soKhach];

    for (int i = 0; i < soKhach; i++) {
        cout << "Ten khach thu " << (i + 1) << ": ";
        cin >> danhSachKhach[i];
    }

    cout << "\nDanh sach khach moi:" << endl;
    for (int i = 0; i < soKhach; i++) {
        cout << "- " << danhSachKhach[i] << endl;
    }

    delete[] danhSachKhach; // dọn dẹp sau khi dùng xong
    return 0;
}
```

Nếu năm nay mời 5 người, năm sau mời 20 người, chương trình vẫn chạy đúng mà không cần sửa code hay khai báo dư thừa — vì kích thước mảng được quyết định linh hoạt lúc chạy, dựa theo input thực tế.

> **Lưu ý cho thi HSG:** trong lập trình thi đấu, người ta thường **ưu tiên dùng `vector`** (sẽ học ở bài STL cơ bản) thay vì `new`/`delete` thủ công, vì vector tự động quản lý bộ nhớ, tránh được lỗi quên `delete`. Tuy nhiên hiểu rõ cấp phát động vẫn rất quan trọng để nắm được cách vector hoạt động bên trong.

## Bài tập gợi ý

1. Viết chương trình nhập `n`, cấp phát động mảng `n` số nguyên, tính tổng rồi giải phóng bộ nhớ.
2. Viết chương trình cấp phát động một mảng 2 chiều (ma trận) kích thước `n x m` (gợi ý: cần cấp phát mảng con trỏ, mỗi con trỏ trỏ tới 1 hàng).
3. Giải thích tại sao đoạn code sau bị lỗi (rò rỉ bộ nhớ): cấp phát `new int[100]` hai lần liên tiếp cho cùng một con trỏ `p` mà không `delete[]` lần cấp phát đầu.
4. Viết chương trình mô phỏng: cấp phát 1 mảng động, sau đó `delete[]` nó, rồi thử truy cập lại phần tử đầu tiên — quan sát hiện tượng gì xảy ra (chạy thử trên máy, có thể không báo lỗi ngay nhưng đây là hành vi không an toàn).
5. (Nâng cao nhẹ) Tìm hiểu vì sao trong C++ hiện đại, người ta khuyến khích dùng "smart pointer" (`unique_ptr`, `shared_ptr`) thay vì `new`/`delete` thủ công.
