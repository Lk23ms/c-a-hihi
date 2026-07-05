# Bài 3: Struct

## 1. Vì sao cần struct?

Đôi khi cần nhóm nhiều biến khác kiểu lại thành 1 khối duy nhất, để dễ quản lý. Ví dụ, một học sinh có: tên (string), tuổi (int), điểm trung bình (double). Thay vì khai báo 3 mảng riêng lẻ, ta gộp thành 1 kiểu dữ liệu mới gọi là `struct`.

## 2. Khai báo và sử dụng struct

```cpp
struct HocSinh {
    string ten;
    int tuoi;
    double diemTB;
};

int main() {
    HocSinh hs1;
    hs1.ten = "Khoi";
    hs1.tuoi = 16;
    hs1.diemTB = 8.5;

    cout << hs1.ten << " - " << hs1.tuoi << " tuoi - diem: " << hs1.diemTB << endl;
    return 0;
}
```

**Giải thích:** dấu chấm (`.`) dùng để truy cập vào từng thành phần (field) bên trong struct. `hs1.ten` nghĩa là "lấy phần `ten` của biến `hs1`".

**Ví dụ dễ hình dung:** struct giống như một tờ **hồ sơ học bạ** in sẵn khung: có ô để điền tên, ô điền tuổi, ô điền điểm. `HocSinh` là "khuôn mẫu tờ hồ sơ", còn `hs1` là "một tờ hồ sơ cụ thể" đã điền thông tin của bạn Khôi vào.

## 3. Khởi tạo struct gọn hơn

```cpp
HocSinh hs2 = {"Lan", 15, 9.0}; // gán giá trị theo đúng thứ tự khai báo field
```

## 4. Mảng struct

Khi cần quản lý nhiều học sinh cùng lúc, dùng mảng của struct:

```cpp
HocSinh danhSach[50];

int n;
cin >> n;

for (int i = 0; i < n; i++) {
    cin >> danhSach[i].ten >> danhSach[i].tuoi >> danhSach[i].diemTB;
}

for (int i = 0; i < n; i++) {
    cout << danhSach[i].ten << ": " << danhSach[i].diemTB << endl;
}
```

**Ví dụ dễ hình dung:** nếu 1 struct là 1 tờ hồ sơ học bạ, thì mảng struct chính là **cả một tủ hồ sơ** chứa hồ sơ của tất cả học sinh trong lớp, xếp theo thứ tự để dễ tra cứu.

## 5. Struct làm tham số hàm

```cpp
void inThongTin(HocSinh hs) {
    cout << hs.ten << " - " << hs.diemTB << endl;
}

// Dùng tham chiếu để tránh sao chép cả struct (tốn bộ nhớ nếu struct lớn)
void capNhatDiem(HocSinh &hs, double diemMoi) {
    hs.diemTB = diemMoi;
}
```

## 6. Struct lồng nhau

```cpp
struct DiaChi {
    string thanhPho;
    string quan;
};

struct HocSinh {
    string ten;
    DiaChi diaChiNha; // struct bên trong struct
};

int main() {
    HocSinh hs;
    hs.ten = "Khoi";
    hs.diaChiNha.thanhPho = "Ho Chi Minh";
    hs.diaChiNha.quan = "Quan 1";
}
```

## 7. Ví dụ thực tế: Quản lý điểm và xếp hạng lớp học

```cpp
#include <iostream>
#include <algorithm>
using namespace std;

struct HocSinh {
    string ten;
    double diemTB;
};

bool soSanh(HocSinh a, HocSinh b) {
    return a.diemTB > b.diemTB; // sắp xếp giảm dần theo điểm
}

int main() {
    int n;
    cout << "So hoc sinh: ";
    cin >> n;

    HocSinh lop[100];
    for (int i = 0; i < n; i++) {
        cout << "Ten va diem hoc sinh " << (i + 1) << ": ";
        cin >> lop[i].ten >> lop[i].diemTB;
    }

    sort(lop, lop + n, soSanh); // sắp xếp theo hàm so sánh tự định nghĩa

    cout << "\n--- Bang xep hang ---" << endl;
    for (int i = 0; i < n; i++) {
        cout << (i + 1) << ". " << lop[i].ten << " - " << lop[i].diemTB << endl;
    }

    return 0;
}
```

Đây chính là cách một phần mềm quản lý điểm thực tế hoạt động: mỗi học sinh là 1 "gói" thông tin (struct), và ta có thể sắp xếp cả danh sách theo bất kỳ tiêu chí nào (ở đây là điểm trung bình) chỉ bằng cách đổi hàm so sánh.

## Bài tập gợi ý

1. Định nghĩa struct `SanPham` gồm tên, giá, số lượng tồn kho. Viết chương trình nhập vào danh sách sản phẩm và tính tổng giá trị hàng tồn kho (giá × số lượng, cộng dồn tất cả sản phẩm).
2. Định nghĩa struct `DiemThi` gồm tên học sinh và 3 điểm (Toán, Văn, Anh). Viết hàm tính điểm trung bình cho 1 struct `DiemThi`.
3. Viết struct `HocSinh` và sắp xếp danh sách học sinh theo tên (thứ tự bảng chữ cái) thay vì theo điểm.
4. Định nghĩa struct `Diem` gồm tọa độ `x, y`. Viết hàm tính khoảng cách giữa 2 điểm (dùng công thức khoảng cách Euclid).
5. (Nâng cao nhẹ) Viết struct `PhanSo` gồm tử số và mẫu số. Viết các hàm cộng, trừ 2 phân số, trả về kết quả cũng là 1 struct `PhanSo` (đã rút gọn nếu có thể — gợi ý: dùng UCLN đã học ở bài Hàm phần Cơ bản để rút gọn).
