# Bài 8: Số Học & Tổ Hợp (Math & Combinatorics)

## 1. Ước chung lớn nhất (UCLN) & bội chung nhỏ nhất (BCNN) — ôn lại và tối ưu

```cpp
int UCLN(int a, int b) {
    if (b == 0) return a;
    return UCLN(b, a % b); // thuật toán Euclid, cực nhanh: O(log(min(a,b)))
}

long long BCNN(int a, int b) {
    return (long long)a / UCLN(a, b) * b; // chia trước để tránh tràn số int
}
```

**Giải thích:** thuật toán Euclid dựa trên tính chất `UCLN(a, b) = UCLN(b, a % b)`, lặp lại cho tới khi `b = 0`. Đây là 1 trong những thuật toán cổ nhất và hiệu quả nhất trong toán học tính toán, chạy cực nhanh dù `a`, `b` lớn tới đâu.

## 2. Sàng nguyên tố Eratosthenes — tìm mọi số nguyên tố tới N cực nhanh

Ở Phần 1, ta đã học kiểm tra 1 số có phải nguyên tố (O(√n) mỗi số). Nếu cần tìm TẤT CẢ số nguyên tố từ 1 đến N, cách hiệu quả hơn nhiều là dùng sàng:

```cpp
vector<bool> laHopSo(1000005, false); // false = có thể là số nguyên tố

void sangNguyenTo(int n) {
    laHopSo[0] = laHopSo[1] = true; // 0 và 1 không phải số nguyên tố

    for (int i = 2; i * i <= n; i++) {
        if (!laHopSo[i]) { // i là số nguyên tố
            for (int j = i * i; j <= n; j += i) {
                laHopSo[j] = true; // đánh dấu mọi bội số của i là hợp số
            }
        }
    }
}
```

**Giải thích:** thay vì kiểm tra từng số một cách độc lập, sàng Eratosthenes "quét" 1 lần: với mỗi số nguyên tố `i` tìm được, đánh dấu luôn TẤT CẢ bội số của nó (`2i, 3i, 4i...`) là hợp số — vì chúng chắc chắn chia hết cho `i` nên không thể là số nguyên tố.

**Ví dụ dễ hình dung:** giống như bạn có danh sách học sinh từ 1 đến 1000, muốn tìm ai "không phải cháu của bất kỳ ai khác trong danh sách". Thay vì tra phả hệ của từng người một cách riêng lẻ (chậm), bạn làm theo cách: chọn 1 người chưa bị đánh dấu, tuyên bố toàn bộ "con cháu" của người đó (bội số) đều bị loại khỏi danh sách "gốc", rồi chuyển sang người tiếp theo chưa bị đánh dấu — quét nhanh hơn nhiều so với tra từng cặp riêng lẻ.

**Độ phức tạp:** O(n log log n) — gần như tuyến tính, nhanh hơn rất nhiều so với kiểm tra từng số một O(n√n).

## 3. Số tổ hợp (Combination) và chỉnh hợp (Permutation)

```cpp
// Tính n! (giai thừa) - đã học đệ quy ở Phần 2, giờ dùng bản lặp cho nhanh
long long giaiThua(int n) {
    long long ketQua = 1;
    for (int i = 2; i <= n; i++) ketQua *= i;
    return ketQua;
}

// Số cách CHỌN k phần tử từ n phần tử, KHÔNG quan tâm thứ tự (tổ hợp)
long long toHop(int n, int k) {
    return giaiThua(n) / (giaiThua(k) * giaiThua(n - k));
}

// Số cách CHỌN VÀ SẮP XẾP k phần tử từ n phần tử, CÓ quan tâm thứ tự (chỉnh hợp)
long long chinhHop(int n, int k) {
    return giaiThua(n) / giaiThua(n - k);
}
```

**Giải thích sự khác biệt giữa tổ hợp và chỉnh hợp — ví dụ dễ hình dung:**
- **Tổ hợp (Combination):** chọn 3 bạn trong lớp 20 bạn để lập 1 **nhóm** làm bài tập — không quan trọng ai được chọn trước, ai sau, chỉ cần đủ 3 người là 1 nhóm. Nhóm {An, Bình, Chi} giống hệt nhóm {Chi, An, Bình} — chỉ tính là 1 cách chọn.
- **Chỉnh hợp (Permutation):** chọn 3 bạn để trao **giải Nhất, Nhì, Ba** — thứ tự cực kỳ quan trọng! An giải Nhất - Bình giải Nhì - Chi giải Ba là HOÀN TOÀN KHÁC với Chi giải Nhất - An giải Nhì - Bình giải Ba, dù cùng 3 người đó.

⚠️ **Lưu ý tràn số:** `n!` tăng cực nhanh (`20! ` đã vượt quá giới hạn `long long`), nên trong thi HSG, người ta thường tính tổ hợp bằng công thức truy hồi tránh tính giai thừa trực tiếp, hoặc tính theo modulo (lấy phần dư cho 1 số nguyên tố lớn) để tránh tràn số.

## 4. Số học modulo — kỹ thuật bắt buộc phải biết khi thi HSG

Khi đáp án bài toán có thể cực lớn (như tính tổ hợp với `n` lớn), đề bài thường yêu cầu "in ra kết quả modulo 10^9 + 7" (lấy phần dư khi chia cho một số nguyên tố lớn), để tránh tràn số.

```cpp
const long long MOD = 1e9 + 7;

long long congMod(long long a, long long b) {
    return (a + b) % MOD;
}

long long nhanMod(long long a, long long b) {
    return (a * b) % MOD;
}
```

**Ví dụ dễ hình dung:** giống như đồng hồ 12 giờ — dù bạn cộng dồn bao nhiêu giờ đi nữa (13h, 25h, 100h...), kim đồng hồ luôn "quay vòng" về lại trong khoảng 0-11. Phép modulo hoạt động y hệt vậy với số học: dù các số trung gian trong quá trình tính toán có lớn cỡ nào, kết quả cuối cùng vẫn luôn được "quay vòng" về trong khoảng `[0, MOD-1]`, giúp con số không bao giờ bị tràn vượt giới hạn lưu trữ của `long long`.

## 5. Ví dụ thực tế: Tính số cách chia đội chơi game

Lớp có 10 bạn, muốn chia ngẫu nhiên thành 1 đội 4 người để thi đấu game với lớp khác, hỏi có bao nhiêu cách chọn đội (không quan trọng vị trí trong đội):

```cpp
#include <iostream>
using namespace std;

long long giaiThua(int n) {
    long long kq = 1;
    for (int i = 2; i <= n; i++) kq *= i;
    return kq;
}

long long toHop(int n, int k) {
    return giaiThua(n) / (giaiThua(k) * giaiThua(n - k));
}

int main() {
    int tongSoBan = 10;
    int soNguoiCanChon = 4;

    cout << "So cach chon doi 4 nguoi tu 10 ban: "
         << toHop(tongSoBan, soNguoiCanChon) << " cach" << endl;
    // Kết quả: C(10,4) = 210 cách

    return 0;
}
```

Đây là ứng dụng thực tế của tổ hợp trong đời sống học sinh — mọi bài toán "chọn nhóm/đội mà không quan tâm thứ tự" đều quy về công thức tổ hợp này, từ chia đội chơi thể thao, chọn ban cán sự lớp, cho tới chọn đại diện đi thi.

## Bài tập gợi ý

1. Cài đặt sàng Eratosthenes tìm mọi số nguyên tố nhỏ hơn 100, in ra danh sách.
2. Tính số cách sắp xếp 5 cuốn sách khác nhau lên 1 kệ sách (đây là bài toán chỉnh hợp/hoán vị đầy đủ — chọn cả 5 trong 5).
3. Lớp có 8 nam và 7 nữ, tính số cách chọn 1 đội gồm 3 nam và 2 nữ (gợi ý: nhân số cách chọn nam với số cách chọn nữ).
4. Viết chương trình tính tổ hợp `C(n, k)` bằng công thức truy hồi Pascal (`C(n,k) = C(n-1,k-1) + C(n-1,k)`) thay vì tính giai thừa trực tiếp — cách này tránh được tràn số với `n` lớn hơn.
5. (Nâng cao nhẹ) Tìm hiểu thuật toán tính lũy thừa nhanh (Fast Exponentiation / Binary Exponentiation) để tính `a^b mod m` trong O(log b) thay vì nhân lặp lại b lần O(b) — kỹ thuật này cực kỳ quan trọng khi `b` lớn cỡ hàng tỷ trong các bài toán số học modulo.
