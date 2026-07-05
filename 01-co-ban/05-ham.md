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

**Ví dụ dễ hình dung:** Truyền theo giá trị giống như bạn đưa cho bạn mình xem 1 tấm ảnh chụp lại của con thú cưng — bạn ấy có vẽ bậy lên tấm ảnh đó thì con thú cưng thật của bạn vẫn nguyên vẹn. Còn truyền theo tham chiếu giống như bạn đưa thẳng con thú cưng thật cho bạn ấy chăm — nếu bạn ấy tắm rửa/cắt lông cho nó, con thú cưng thật của bạn cũng thay đổi theo luôn, vì đó chính là "bản gốc" chứ không phải bản sao.

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

**Giải thích:** cả 3 hàm đều tên là `tong`, nhưng C++ tự phân biệt được nên gọi hàm nào dựa vào **số lượng và kiểu tham số** bạn truyền vào lúc gọi. Gọi `tong(2, 3)` sẽ chạy hàm đầu tiên, gọi `tong(2.5, 3.5)` sẽ chạy hàm thứ hai, gọi `tong(1, 2, 3)` sẽ chạy hàm thứ ba.

**Ví dụ dễ hình dung:** giống như từ "rót" trong đời sống — "rót nước" và "rót trà" nghe tên hành động giống nhau, nhưng tùy vào thứ bạn cầm trên tay (nước hay trà) mà người nghe tự hiểu bạn đang muốn làm gì. Hàm nạp chồng cũng vậy: cùng cái tên `tong`, nhưng "hình dạng" tham số truyền vào sẽ quyết định phiên bản nào được chạy.

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

## 8. Ví dụ thực tế: Tính tiền cước xe công nghệ (kiểu Grab)

Các app đặt xe tính tiền dựa trên khoảng cách di chuyển: có mức giá mở cửa cố định, cộng thêm tiền theo từng km tiếp theo. Đóng gói việc tính giá vào 1 hàm giúp code gọn và dễ tái sử dụng:

```cpp
#include <iostream>
using namespace std;

double tinhTienXe(double km) {
    double giaMoCua = 12000;  // giá cho 2km đầu
    double giaMoiKm = 4000;   // giá mỗi km tiếp theo

    if (km <= 2) {
        return giaMoCua;
    }
    return giaMoCua + (km - 2) * giaMoiKm;
}

int main() {
    double quangDuong;
    cout << "Nhap quang duong (km): ";
    cin >> quangDuong;

    cout << "Tien cuoc: " << tinhTienXe(quangDuong) << " dong" << endl;
    return 0;
}
```

Đi 1.5km → chỉ tính giá mở cửa 12000. Đi 7km → 12000 + (7-2)*4000 = 32000. Nhờ tách thành hàm `tinhTienXe`, sau này nếu muốn tính cho nhiều chuyến đi khác nhau, chỉ cần gọi lại hàm này nhiều lần, không phải viết lại công thức mỗi lần.

## Ví dụ thực tế 2: Máy tính đơn giản dùng hàm

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
