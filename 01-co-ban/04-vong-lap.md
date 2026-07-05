# Bài 4: Vòng Lặp

## 1. Vòng lặp `for`

Dùng khi biết trước số lần lặp.

```cpp
for (int i = 1; i <= 5; i++) {
    cout << i << " ";
}
// In ra: 1 2 3 4 5
```

Cấu trúc: `for (khởi tạo; điều kiện; bước nhảy)`

```cpp
for (int i = 10; i >= 1; i--) { ... }     // lặp giảm dần
for (int i = 0; i < 20; i += 2) { ... }   // lặp bước 2
```

## 2. Vòng lặp `while`

Dùng khi chưa biết trước số lần lặp, lặp khi điều kiện còn đúng.

```cpp
int n;
cin >> n;

int dem = 0;
while (n > 0) {
    dem++;
    n /= 10;
}
cout << "So chu so: " << dem << endl;
```

## 3. Vòng lặp `do-while`

Giống `while` nhưng luôn chạy ít nhất 1 lần trước khi kiểm tra điều kiện.

```cpp
int x;
do {
    cout << "Nhap so duong: ";
    cin >> x;
} while (x <= 0);
```

## 4. `break` và `continue`

- `break`: thoát khỏi vòng lặp ngay lập tức.
- `continue`: bỏ qua phần còn lại của lần lặp hiện tại, chuyển sang lần lặp tiếp theo.

```cpp
for (int i = 1; i <= 10; i++) {
    if (i == 5) break;       // dừng khi i = 5
    cout << i << " ";
}
// In ra: 1 2 3 4

for (int i = 1; i <= 10; i++) {
    if (i % 2 == 0) continue; // bỏ qua số chẵn
    cout << i << " ";
}
// In ra: 1 3 5 7 9
```

## 5. Vòng lặp lồng nhau (nested loop)

Thường dùng để duyệt bảng, ma trận, hoặc in hình.

```cpp
// In bảng cửu chương từ 2 đến 5
for (int i = 2; i <= 5; i++) {
    for (int j = 1; j <= 10; j++) {
        cout << i << " x " << j << " = " << i * j << endl;
    }
}
```

```cpp
// In tam giác sao
for (int i = 1; i <= 5; i++) {
    for (int j = 1; j <= i; j++) {
        cout << "* ";
    }
    cout << endl;
}
/*
*
* *
* * *
* * * *
* * * * *
*/
```

## 6. Ví dụ thực tế: Kiểm tra số nguyên tố

```cpp
#include <iostream>
using namespace std;

int main() {
    int n;
    cin >> n;

    bool laNguyenTo = (n >= 2);
    for (int i = 2; i * i <= n; i++) {
        if (n % i == 0) {
            laNguyenTo = false;
            break;
        }
    }

    cout << (laNguyenTo ? "La so nguyen to" : "Khong phai so nguyen to") << endl;
    return 0;
}
```

> **Mẹo thi HSG:** Khi kiểm tra số nguyên tố, chỉ cần duyệt ước từ `2` đến `sqrt(n)` (viết là `i * i <= n` để tránh dùng hàm `sqrt`), giúp thuật toán chạy nhanh hơn nhiều so với duyệt đến `n`.

## Bài tập gợi ý

1. Tính tổng các số từ 1 đến `n` bằng vòng lặp `for`, sau đó so sánh với công thức `n*(n+1)/2`.
2. Viết chương trình in ra tất cả số nguyên tố nhỏ hơn `n` (dùng lại ý tưởng kiểm tra nguyên tố ở trên).
3. Tính giai thừa của `n` (`n!`) bằng vòng lặp `while`.
4. In ra dãy Fibonacci gồm `n` số đầu tiên (0, 1, 1, 2, 3, 5, 8, ...).
5. (Nâng cao nhẹ) Viết chương trình đếm xem trong khoảng từ 1 đến `n`, có bao nhiêu số là "số hoàn hảo" (perfect number — tổng các ước số dương nhỏ hơn nó bằng chính nó, ví dụ 6 = 1+2+3).
