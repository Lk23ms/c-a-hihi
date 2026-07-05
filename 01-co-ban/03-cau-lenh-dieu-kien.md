# Bài 3: Câu Lệnh Điều Kiện

## 1. if / else

```cpp
int diem;
cin >> diem;

if (diem >= 5) {
    cout << "Dat" << endl;
} else {
    cout << "Khong dat" << endl;
}
```

## 2. if / else if / else — nhiều nhánh

```cpp
int diem;
cin >> diem;

if (diem >= 9) {
    cout << "Xuat sac" << endl;
} else if (diem >= 8) {
    cout << "Gioi" << endl;
} else if (diem >= 6.5) {
    cout << "Kha" << endl;
} else if (diem >= 5) {
    cout << "Trung binh" << endl;
} else {
    cout << "Yeu" << endl;
}
```

⚠️ Thứ tự các điều kiện quan trọng: phải kiểm tra từ cao xuống thấp (hoặc thấp lên cao) một cách nhất quán, nếu không sẽ sai logic.

## 3. Toán tử điều kiện ba ngôi (ternary)

Cách viết gọn cho if/else đơn giản:
```cpp
int a = 5, b = 3;
int max = (a > b) ? a : b;   // nếu a > b thì max = a, ngược lại max = b
```

## 4. switch-case

Dùng khi so sánh một biến với nhiều giá trị cụ thể (không dùng được cho khoảng giá trị).

```cpp
int thu;
cin >> thu;

switch (thu) {
    case 2:
        cout << "Thu Hai" << endl;
        break;
    case 3:
        cout << "Thu Ba" << endl;
        break;
    case 7:
        cout << "Chu Nhat" << endl;
        break;
    default:
        cout << "Khong hop le" << endl;
}
```

⚠️ Luôn nhớ `break;` sau mỗi `case`, nếu không chương trình sẽ "rơi xuống" (fall-through) chạy tiếp case bên dưới.

## 5. Lồng điều kiện (nested if)

```cpp
int tuoi;
bool coVe;
cin >> tuoi >> coVe;

if (coVe) {
    if (tuoi < 6) {
        cout << "Mien phi" << endl;
    } else {
        cout << "Tinh phi day du" << endl;
    }
} else {
    cout << "Khong duoc vao" << endl;
}
```

## 6. Ví dụ thực tế: Tính giá vé xem phim theo tuổi

Rạp phim thường có giá vé khác nhau tùy độ tuổi: trẻ em, người lớn, người cao tuổi được giảm giá:

```cpp
#include <iostream>
using namespace std;

int main() {
    int tuoi;
    cout << "Nhap tuoi: ";
    cin >> tuoi;

    int giaVe;
    if (tuoi < 6) {
        giaVe = 0; // trẻ dưới 6 tuổi miễn phí
    } else if (tuoi <= 12) {
        giaVe = 40000; // trẻ em
    } else if (tuoi <= 60) {
        giaVe = 80000; // người lớn
    } else {
        giaVe = 50000; // người cao tuổi, giảm giá
    }

    cout << "Gia ve cua ban: " << giaVe << " dong" << endl;
    return 0;
}
```

Thử với tuổi 10 → 40000 đồng (trẻ em), tuổi 25 → 80000 đồng (người lớn), tuổi 70 → 50000 đồng (giảm giá cao tuổi). Đây chính xác là cách các app đặt vé xem phim tính giá cho bạn mỗi khi chọn ngày sinh.

## Ví dụ thực tế 2: Phân loại BMI

```cpp
#include <iostream>
using namespace std;

int main() {
    double canNang, chieuCao; // kg, m
    cin >> canNang >> chieuCao;

    double bmi = canNang / (chieuCao * chieuCao);

    if (bmi < 18.5)
        cout << "Thieu can" << endl;
    else if (bmi < 25)
        cout << "Binh thuong" << endl;
    else if (bmi < 30)
        cout << "Thua can" << endl;
    else
        cout << "Beo phi" << endl;

    return 0;
}
```

## Bài tập gợi ý

1. Viết chương trình nhập 3 số, tìm và in ra số lớn nhất bằng `if/else`.
2. Viết chương trình phân loại tam giác dựa vào 3 cạnh: đều, cân, vuông, hay thường (dùng lồng `if`).
3. Dùng `switch-case` viết chương trình nhập số tháng (1-12), in ra số ngày của tháng đó (lưu ý tháng 2 có 28/29 ngày).
4. Viết chương trình nhập điểm 3 môn Toán, Văn, Anh. In ra "Đậu" nếu cả 3 môn đều >= 5 VÀ điểm trung bình >= 6.5, ngược lại in "Rớt".
5. (Nâng cao nhẹ) Viết chương trình giải phương trình bậc 2 `ax² + bx + c = 0`, xét đầy đủ các trường hợp: vô nghiệm, nghiệm kép, 2 nghiệm phân biệt, và trường hợp đặc biệt khi `a = 0` (phương trình bậc 1).
