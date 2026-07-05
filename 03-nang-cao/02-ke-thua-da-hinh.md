# Bài 2: Kế Thừa & Đa Hình

## 1. Kế thừa (Inheritance) là gì?

Kế thừa cho phép một class "thừa hưởng" toàn bộ dữ liệu và phương thức từ một class khác, rồi có thể bổ sung thêm hoặc thay đổi chút ít — tránh phải viết lại code trùng lặp.

```cpp
class DongVat {
public:
    string ten;

    DongVat(string t) {
        ten = t;
    }

    void an() {
        cout << ten << " dang an." << endl;
    }
};

class Cho : public DongVat {  // Cho "kế thừa" từ DongVat
public:
    Cho(string t) : DongVat(t) {} // gọi constructor của lớp cha

    void sua() {  // phương thức riêng, chỉ Cho mới có
        cout << ten << " sua: Gau gau!" << endl;
    }
};

int main() {
    Cho conCho("Mi Lu");
    conCho.an();   // thừa hưởng từ DongVat -> "Mi Lu dang an."
    conCho.sua();  // riêng của Cho -> "Mi Lu sua: Gau gau!"
}
```

**Giải thích:** `class Cho : public DongVat` nghĩa là "Cho kế thừa DongVat". `Cho` tự động có luôn biến `ten` và phương thức `an()` từ `DongVat` mà không cần viết lại, đồng thời có thêm phương thức `sua()` riêng của mình.

**Ví dụ dễ hình dung:** giống mối quan hệ **cha truyền con nối trong 1 gia đình làm nghề mộc**. Người con (`Cho`) tự động thừa hưởng những kỹ năng cơ bản từ người cha (`DongVat`, đại diện cho "tất cả động vật nói chung" — biết ăn), nhưng người con có thể học thêm kỹ năng riêng của mình (`sua()` — chỉ có ở loài chó, không phải mọi động vật đều biết sủa).

## 2. Từ vựng quan trọng: lớp cha (base class) và lớp con (derived class)

- **Lớp cha / lớp cơ sở (base class):** `DongVat` — chứa các đặc điểm chung.
- **Lớp con / lớp dẫn xuất (derived class):** `Cho` — kế thừa đặc điểm chung, có thể thêm đặc điểm riêng.

## 3. Đa hình (Polymorphism) qua hàm ảo (virtual function)

Đa hình cho phép các lớp con **định nghĩa lại (override)** một phương thức của lớp cha theo cách riêng của mình, nhưng vẫn gọi qua cùng 1 tên hàm.

```cpp
class DongVat {
public:
    string ten;
    DongVat(string t) { ten = t; }

    virtual void keu() {  // từ khóa "virtual" cho phép lớp con ghi đè
        cout << ten << " keu mot am thanh nao do." << endl;
    }
};

class Cho : public DongVat {
public:
    Cho(string t) : DongVat(t) {}
    void keu() override {  // ghi đè lại phương thức keu() của lớp cha
        cout << ten << ": Gau gau!" << endl;
    }
};

class Meo : public DongVat {
public:
    Meo(string t) : DongVat(t) {}
    void keu() override {
        cout << ten << ": Meo meo!" << endl;
    }
};

int main() {
    DongVat *ds[2];
    ds[0] = new Cho("Mi Lu");
    ds[1] = new Meo("Mimi");

    for (int i = 0; i < 2; i++) {
        ds[i]->keu(); // gọi cùng 1 tên hàm, nhưng mỗi con vật "kêu" khác nhau
    }
    // In ra: "Mi Lu: Gau gau!" và "Mimi: Meo meo!"

    delete ds[0];
    delete ds[1];
    return 0;
}
```

**Giải thích:** dù `ds[0]` và `ds[1]` đều được khai báo chung kiểu con trỏ `DongVat*`, nhưng khi gọi `keu()`, chương trình tự biết phải chạy đúng phiên bản `keu()` của `Cho` hay `Meo` tùy vào con vật thực sự đang được trỏ tới — đây chính là ý nghĩa của "đa hình" (cùng 1 lời gọi hàm, nhưng có nhiều hình thái kết quả khác nhau).

**Ví dụ dễ hình dung:** giống như bạn hô to "Mọi người, hãy chào hỏi!" trong 1 lớp học đa quốc gia — cùng 1 mệnh lệnh ("chào hỏi") nhưng bạn Nhật sẽ cúi đầu, bạn Việt sẽ khoanh tay, bạn Mỹ sẽ vẫy tay. Mỗi người "thực hiện" hành động theo cách riêng của văn hóa mình, dù nhận chung 1 mệnh lệnh — đó chính là tinh thần đa hình.

## 4. Vì sao cần `virtual`?

Nếu bỏ từ khóa `virtual` ở hàm `keu()` của lớp cha, chương trình sẽ luôn gọi phiên bản `keu()` của `DongVat` (lớp cha) bất kể con trỏ đang trỏ tới `Cho` hay `Meo` — mất đi khả năng "tùy biến theo loại con vật thực sự". `virtual` chính là từ khóa "bật công tắc" cho phép đa hình hoạt động.

## 5. Ví dụ thực tế: Hệ thống tính lương nhân viên theo từng loại hình

```cpp
#include <iostream>
using namespace std;

class NhanVien {
public:
    string ten;
    NhanVien(string t) { ten = t; }

    virtual double tinhLuong() {
        return 5000000; // lương cơ bản mặc định
    }
};

class NhanVienChinhThuc : public NhanVien {
public:
    NhanVienChinhThuc(string t) : NhanVien(t) {}
    double tinhLuong() override {
        return 10000000; // lương cố định cao hơn
    }
};

class NhanVienThoiVu : public NhanVien {
public:
    int soNgayLam;
    NhanVienThoiVu(string t, int ngay) : NhanVien(t) {
        soNgayLam = ngay;
    }
    double tinhLuong() override {
        return soNgayLam * 300000; // trả theo ngày công
    }
};

int main() {
    NhanVien *dsNhanVien[2];
    dsNhanVien[0] = new NhanVienChinhThuc("Khoi");
    dsNhanVien[1] = new NhanVienThoiVu("Lan", 15);

    for (int i = 0; i < 2; i++) {
        cout << dsNhanVien[i]->ten << " nhan luong: "
             << dsNhanVien[i]->tinhLuong() << " dong" << endl;
    }

    delete dsNhanVien[0];
    delete dsNhanVien[1];
    return 0;
}
```

Đây chính là cách phần mềm tính lương thực tế xử lý nhiều loại nhân viên khác nhau (chính thức, thời vụ, thực tập...) — mỗi loại có công thức tính lương riêng, nhưng chương trình chính chỉ cần gọi chung 1 lệnh `tinhLuong()` cho mọi nhân viên mà không cần biết trước đó là loại nào.

## Bài tập gợi ý

1. Viết class `HinhHoc` (lớp cha) với phương thức ảo `tinhDienTich()`. Viết 2 lớp con `HinhVuong` và `HinhTron` ghi đè phương thức này theo công thức riêng.
2. Viết class `PhuongTien` (lớp cha) có phương thức ảo `diChuyen()`. Viết các lớp con `XeMay`, `XeDap`, `MayBay` ghi đè lại với thông báo khác nhau (ví dụ "XeMay chay tren duong", "MayBay bay tren khong").
3. Giải thích tại sao nếu bỏ từ khóa `virtual` ở bài tập 1, chương trình sẽ luôn tính diện tích theo công thức của `HinhHoc` (lớp cha) dù object thực sự là `HinhVuong` hay `HinhTron`.
4. Viết class `TaiKhoan` (lớp cha, đã học ở bài trước) và lớp con `TaiKhoanVIP` kế thừa, có thêm phương thức `napTien` ghi đè để cộng thêm 10% tiền thưởng mỗi lần nạp.
5. (Nâng cao nhẹ) Tìm hiểu khái niệm "lớp trừu tượng" (abstract class) trong C++ — một class có ít nhất 1 hàm ảo thuần túy (`virtual void ham() = 0;`) và không thể tạo object trực tiếp từ nó. Vì sao khái niệm này hữu ích khi thiết kế các hệ thống lớn?
