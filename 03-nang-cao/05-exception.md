# Bài 5: Xử Lý Ngoại Lệ (Exception Handling)

## 1. Vấn đề: chương trình bị crash khi gặp lỗi bất ngờ

```cpp
int a = 10, b = 0;
cout << a / b << endl; // chia cho 0 -> chương trình crash ngay lập tức!
```

Trong thực tế, nhiều tình huống lỗi không thể lường trước hết được lúc viết code (người dùng nhập sai định dạng, chia cho 0, truy cập file không tồn tại...). Nếu để chương trình crash đột ngột, trải nghiệm người dùng rất tệ. Exception handling giúp "bắt lỗi" và xử lý nó một cách có kiểm soát thay vì để chương trình sập.

## 2. `try`, `throw`, `catch` — 3 từ khóa cốt lõi

```cpp
#include <iostream>
using namespace std;

double chia(double a, double b) {
    if (b == 0) {
        throw runtime_error("Khong the chia cho 0!"); // "ném" ra 1 lỗi
    }
    return a / b;
}

int main() {
    try {
        cout << chia(10, 2) << endl; // chạy bình thường -> in ra 5
        cout << chia(10, 0) << endl; // gây lỗi -> nhảy thẳng xuống catch
        cout << "Dong nay se khong bao gio chay" << endl;
    } catch (runtime_error &e) {
        cout << "Da bat duoc loi: " << e.what() << endl;
    }

    cout << "Chuong trinh van tiep tuc chay binh thuong sau do." << endl;
    return 0;
}
```

**Giải thích:**
- `try { ... }`: khối code "thử chạy", nếu có lỗi xảy ra bên trong, nó sẽ ngay lập tức nhảy tới khối `catch` tương ứng, bỏ qua toàn bộ phần code còn lại trong `try`.
- `throw`: "ném" ra một lỗi khi phát hiện tình huống bất thường.
- `catch`: "bắt lấy" lỗi vừa được ném ra, xử lý nó theo cách mình muốn (ở đây là in thông báo), thay vì để chương trình crash.

**Ví dụ dễ hình dung:** giống như đi xiếc có lưới an toàn bên dưới — `try` là "màn biểu diễn thử" trên dây, nếu diễn viên (đoạn code) lỡ trượt chân (gặp lỗi), họ sẽ `throw` bản thân xuống, và `catch` chính là tấm lưới an toàn bắt lấy họ, giúp họ không bị rơi tự do (chương trình không bị crash) mà có thể đứng dậy tiếp tục show diễn (chương trình tiếp tục chạy).

## 3. Nhiều loại lỗi khác nhau (nhiều `catch`)

```cpp
try {
    // ... code có thể gây nhiều loại lỗi khác nhau
    throw invalid_argument("Du lieu dau vao khong hop le");
}
catch (invalid_argument &e) {
    cout << "Loi du lieu dau vao: " << e.what() << endl;
}
catch (runtime_error &e) {
    cout << "Loi runtime: " << e.what() << endl;
}
catch (...) { // bắt mọi loại lỗi còn lại chưa được liệt kê ở trên
    cout << "Co loi khong xac dinh xay ra" << endl;
}
```

## 4. Tự định nghĩa loại lỗi riêng

```cpp
class SoDuKhongDuException : public runtime_error {
public:
    SoDuKhongDuException() : runtime_error("So du trong tai khoan khong du!") {}
};

class TaiKhoan {
private:
    double soDu;
public:
    TaiKhoan(double sd) { soDu = sd; }

    void rutTien(double soTien) {
        if (soTien > soDu) {
            throw SoDuKhongDuException(); // ném ra lỗi tùy chỉnh
        }
        soDu -= soTien;
    }
};

int main() {
    TaiKhoan tk(100000);
    try {
        tk.rutTien(500000); // vượt quá số dư
    } catch (SoDuKhongDuException &e) {
        cout << "Giao dich that bai: " << e.what() << endl;
    }
}
```

**Ví dụ dễ hình dung:** giống việc đặt tên riêng cho từng loại "biển báo cảnh báo" trong nhà máy — thay vì chỉ có 1 biển báo chung chung "Nguy hiểm!", bạn có biển riêng "Cảnh báo cháy nổ", "Cảnh báo điện giật"... giúp người xử lý biết chính xác cần phản ứng thế nào với từng tình huống cụ thể.

## 5. Ví dụ thực tế: Máy ATM xử lý các lỗi giao dịch

```cpp
#include <iostream>
#include <stdexcept>
using namespace std;

class ATM {
private:
    double soDuTaiKhoan;

public:
    ATM(double soDuBanDau) {
        soDuTaiKhoan = soDuBanDau;
    }

    void rutTien(double soTien) {
        if (soTien <= 0) {
            throw invalid_argument("So tien rut phai lon hon 0!");
        }
        if (soTien % 50000 != 0) {
            throw invalid_argument("May ATM chi rut duoc boi so cua 50,000!");
        }
        if (soTien > soDuTaiKhoan) {
            throw runtime_error("So du tai khoan khong du!");
        }

        soDuTaiKhoan -= soTien;
        cout << "Rut thanh cong " << soTien << " dong. So du con lai: "
             << soDuTaiKhoan << endl;
    }
};

int main() {
    ATM maATM(1000000);

    double cacGiaoDich[] = {200000, -50000, 123456, 5000000};

    for (double soTien : cacGiaoDich) {
        try {
            maATM.rutTien(soTien);
        } catch (invalid_argument &e) {
            cout << "Giao dich " << soTien << " bi tu choi: " << e.what() << endl;
        } catch (runtime_error &e) {
            cout << "Giao dich " << soTien << " that bai: " << e.what() << endl;
        }
    }

    return 0;
}
```

Nhờ exception handling, dù giao dịch thứ 2, 3, 4 đều gặp lỗi khác nhau (số âm, không chia hết 50000, vượt số dư), chương trình vẫn tiếp tục xử lý các giao dịch tiếp theo bình thường thay vì bị crash ngay từ giao dịch lỗi đầu tiên — đúng như cách máy ATM thật hoạt động ngoài đời.

## Bài tập gợi ý

1. Viết hàm `chiaAntoan(int a, int b)` ném ra lỗi `runtime_error` khi `b == 0`, và bắt lỗi đó ở `main()` để in thông báo thân thiện thay vì để chương trình crash.
2. Viết chương trình nhập tuổi từ bàn phím, ném ra lỗi `invalid_argument` nếu tuổi nhập vào là số âm hoặc lớn hơn 150.
3. Viết class `NganHang` với phương thức `chuyenTien` ném ra 2 loại lỗi khác nhau: `SoDuKhongDuException` (không đủ tiền) và `SoTienKhongHopLeException` (số tiền âm hoặc bằng 0).
4. Viết chương trình mô phỏng nhập điểm thi (0-10), dùng try-catch để bắt lỗi khi người dùng nhập điểm ngoài khoảng hợp lệ, và cho phép nhập lại cho tới khi đúng.
5. (Nâng cao nhẹ) Tìm hiểu khối `finally`-tương-đương trong C++ (C++ không có từ khóa `finally` như Java, nhưng có kỹ thuật RAII để đảm bảo dọn dẹp tài nguyên dù có lỗi xảy ra hay không) — vì sao kỹ thuật này quan trọng khi làm việc với file hoặc kết nối mạng?
