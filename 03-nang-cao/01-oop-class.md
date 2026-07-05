# Bài 1: Lập Trình Hướng Đối Tượng (OOP) — Class & Object

## 1. Vì sao cần OOP?

Ở bài Struct, ta đã nhóm dữ liệu lại thành 1 khối (ví dụ `HocSinh` gồm tên, tuổi, điểm). Nhưng struct chỉ chứa **dữ liệu**, không chứa **hành vi** (hàm xử lý) đi kèm. `class` giải quyết điều này: gộp cả dữ liệu và hàm xử lý dữ liệu đó vào chung 1 "khuôn mẫu".

## 2. Khai báo class cơ bản

```cpp
class HocSinh {
public:
    string ten;
    int tuoi;
    double diemTB;

    void inThongTin() {
        cout << ten << " - " << tuoi << " tuoi - diem: " << diemTB << endl;
    }
};

int main() {
    HocSinh hs;
    hs.ten = "Khoi";
    hs.tuoi = 16;
    hs.diemTB = 8.5;

    hs.inThongTin(); // gọi hàm (gọi là "phương thức") của object
    return 0;
}
```

**Giải thích:** `class HocSinh` là "bản thiết kế", còn `hs` là "sản phẩm thực tế" được tạo ra từ bản thiết kế đó — gọi là **object** (đối tượng). Hàm `inThongTin()` nằm bên trong class được gọi là **phương thức (method)** — nó là hành vi gắn liền với object.

**Ví dụ dễ hình dung:** class giống như **bản vẽ thiết kế một chiếc xe hơi**, quy định xe có những bộ phận gì (bánh xe, động cơ, màu sơn — tương ứng với dữ liệu) và xe làm được gì (chạy, bấm còi, bật đèn — tương ứng với phương thức). Mỗi chiếc xe hơi cụ thể được sản xuất ra từ bản vẽ đó (mỗi chiếc là 1 object) có thể có màu sơn khác nhau, nhưng đều có chung khả năng chạy/bấm còi như thiết kế quy định.

## 3. `public` và `private` — che giấu dữ liệu

```cpp
class TaiKhoanNganHang {
private:
    double soDu; // không cho truy cập trực tiếp từ bên ngoài

public:
    TaiKhoanNganHang(double soDuBanDau) { // hàm khởi tạo (constructor)
        soDu = soDuBanDau;
    }

    void napTien(double soTien) {
        if (soTien > 0) soDu += soTien;
    }

    void rutTien(double soTien) {
        if (soTien > 0 && soTien <= soDu) {
            soDu -= soTien;
        } else {
            cout << "Khong du tien de rut!" << endl;
        }
    }

    double xemSoDu() {
        return soDu;
    }
};

int main() {
    TaiKhoanNganHang taiKhoan(100000); // tạo object, gọi constructor với 100000

    taiKhoan.napTien(50000);
    taiKhoan.rutTien(30000);

    cout << "So du hien tai: " << taiKhoan.xemSoDu() << endl; // 120000

    // taiKhoan.soDu = 999999999; // LỖI! không được phép vì soDu là private
}
```

**Giải thích:** `private` nghĩa là chỉ các phương thức **bên trong** class mới truy cập được biến đó, thế giới bên ngoài (như trong `main()`) không thể sửa trực tiếp. `public` là các phương thức/dữ liệu mà bên ngoài được phép gọi tới.

**Ví dụ dễ hình dung:** giống như tài khoản ngân hàng thật — bạn không thể tự ý mở két sắt của ngân hàng và sửa số dư của mình (đó là `private`). Bạn chỉ có thể tương tác qua các "cổng giao dịch" được cho phép: nộp tiền, rút tiền, xem số dư (đó là các phương thức `public`). Việc "che giấu" dữ liệu bên trong và chỉ cho tương tác qua cổng an toàn gọi là tính **đóng gói (encapsulation)** — một trong những ý tưởng cốt lõi của OOP.

## 4. Constructor (hàm khởi tạo)

Constructor là hàm đặc biệt tự động chạy khi object được tạo ra, thường dùng để gán giá trị ban đầu:

```cpp
class HocSinh {
public:
    string ten;
    int tuoi;

    HocSinh() { // constructor mặc định (không tham số)
        ten = "Chua co ten";
        tuoi = 0;
    }

    HocSinh(string t, int tu) { // constructor có tham số
        ten = t;
        tuoi = tu;
    }
};

int main() {
    HocSinh hs1;              // gọi constructor không tham số
    HocSinh hs2("Lan", 15);   // gọi constructor có tham số
}
```

## 5. Ví dụ thực tế: Class quản lý nhân vật trong game

```cpp
#include <iostream>
using namespace std;

class NhanVat {
private:
    string ten;
    int mauMax;
    int mauHienTai;

public:
    NhanVat(string t, int mau) {
        ten = t;
        mauMax = mau;
        mauHienTai = mau;
    }

    void nhanSatThuong(int soLuong) {
        mauHienTai -= soLuong;
        if (mauHienTai < 0) mauHienTai = 0;
        cout << ten << " bi mat " << soLuong << " mau!" << endl;
    }

    void hoiMau(int soLuong) {
        mauHienTai += soLuong;
        if (mauHienTai > mauMax) mauHienTai = mauMax;
        cout << ten << " hoi " << soLuong << " mau!" << endl;
    }

    void hienThiTrangThai() {
        cout << ten << ": " << mauHienTai << "/" << mauMax << " HP" << endl;
    }

    bool coConSong() {
        return mauHienTai > 0;
    }
};

int main() {
    NhanVat nguoiChoi("Chien Binh", 100);

    nguoiChoi.hienThiTrangThai();  // Chien Binh: 100/100 HP
    nguoiChoi.nhanSatThuong(30);   // mất 30 máu
    nguoiChoi.hienThiTrangThai();  // Chien Binh: 70/100 HP
    nguoiChoi.hoiMau(15);          // hồi 15 máu
    nguoiChoi.hienThiTrangThai();  // Chien Binh: 85/100 HP

    return 0;
}
```

Đây chính xác là cách các game thật quản lý nhân vật: mọi thông tin (máu, tên) và hành vi (nhận sát thương, hồi máu) đều được gói gọn trong 1 class `NhanVat` — cần thêm nhân vật mới chỉ cần tạo thêm object mới từ class này, không cần viết lại logic từ đầu.

## Bài tập gợi ý

1. Viết class `HinhChuNhat` gồm chiều dài, chiều rộng (private), và các phương thức tính chu vi, diện tích (public).
2. Viết class `ThoiGian` gồm giờ, phút, giây. Viết phương thức `congGiay(int soGiay)` để cộng thêm số giây vào, tự động xử lý tràn (60 giây = 1 phút, 60 phút = 1 giờ).
3. Viết class `GioHang` (giỏ hàng) có thể thêm sản phẩm (tên + giá), tính tổng tiền, và hiển thị danh sách sản phẩm.
4. Viết class `Diem` (điểm tọa độ x, y) với constructor, và phương thức tính khoảng cách tới 1 điểm `Diem` khác.
5. (Nâng cao nhẹ) Viết class `HangDoi` (Queue) tự cài đặt bằng mảng bên trong, với các phương thức `themVao`, `layRa`, `xemDauHang`, mô phỏng lại 1 hàng đợi thực tế (giống xếp hàng mua vé) mà không dùng thư viện `queue` có sẵn.
