# Bài 4: Quy Hoạch Động (Dynamic Programming)

## 1. Vấn đề: đệ quy thô tính đi tính lại quá nhiều lần

Nhớ lại hàm Fibonacci đệ quy ở Phần 2:
```cpp
int fibonacci(int n) {
    if (n <= 1) return n;
    return fibonacci(n - 1) + fibonacci(n - 2);
}
```

Để tính `fibonacci(5)`, hàm phải tính `fibonacci(3)` những 2 lần, `fibonacci(2)` tận 3 lần... — cực kỳ lãng phí. Với `n = 40`, thuật toán này có thể mất vài giây; với `n = 50`, có thể mất... vài phút! Quy hoạch động (DP) giải quyết đúng vấn đề này: **lưu lại kết quả đã tính, không tính lại lần nữa**.

## 2. Memoization (ghi nhớ) — cách dễ nhất để bắt đầu học DP

```cpp
int memo[100]; // mảng lưu lại kết quả đã tính, -1 nghĩa là "chưa tính"

int fibonacci(int n) {
    if (n <= 1) return n;
    if (memo[n] != -1) return memo[n]; // đã tính rồi, dùng lại luôn, không tính nữa

    memo[n] = fibonacci(n - 1) + fibonacci(n - 2); // tính và LƯU LẠI kết quả
    return memo[n];
}

int main() {
    memset(memo, -1, sizeof(memo)); // khởi tạo toàn bộ mảng memo là -1
    cout << fibonacci(40) << endl;  // giờ chạy cực nhanh, dù n=40
}
```

**Giải thích:** đây gọi là "Top-down DP" (từ trên xuống) — vẫn viết đệ quy như cũ, chỉ thêm 1 bước kiểm tra "đã tính chưa" trước khi tính lại, và lưu kết quả vào mảng `memo` sau khi tính xong.

**Ví dụ dễ hình dung:** giống như khi giải bài tập về nhà, nếu bạn đã giải xong 1 câu và ghi đáp án ra giấy nháp, lần sau gặp lại đúng câu đó, bạn chỉ cần nhìn lại tờ giấy nháp thay vì giải lại từ đầu — tờ giấy nháp đó chính là mảng `memo`.

## 3. Tabulation (lập bảng) — cách viết DP "xuôi" từ dưới lên

```cpp
int fibonacci(int n) {
    vector<int> dp(n + 1);
    dp[0] = 0;
    dp[1] = 1;

    for (int i = 2; i <= n; i++) {
        dp[i] = dp[i - 1] + dp[i - 2]; // xây dựng kết quả lớn hơn từ kết quả nhỏ đã biết
    }

    return dp[n];
}
```

**Giải thích:** thay vì đệ quy "từ trên xuống", cách này xây bảng `dp[]` "từ dưới lên": tính `dp[0]`, `dp[1]` trước (là các giá trị gốc, base case), rồi lần lượt tính `dp[2]`, `dp[3]`... dựa vào các giá trị đã biết trước đó, cho tới khi tới `dp[n]`.

## 4. 3 bước để giải 1 bài toán DP

1. **Định nghĩa trạng thái:** `dp[i]` đại diện cho cái gì? (ví dụ: `dp[i]` là kết quả Fibonacci thứ `i`)
2. **Tìm công thức truy hồi:** `dp[i]` được tính từ những `dp[j]` nào trước đó? (ví dụ: `dp[i] = dp[i-1] + dp[i-2]`)
3. **Xác định trạng thái gốc (base case):** giá trị khởi đầu là gì? (ví dụ: `dp[0] = 0, dp[1] = 1`)

## 5. Bài toán kinh điển: Cái túi 0/1 (Knapsack)

Có `n` món đồ, mỗi món có trọng lượng và giá trị riêng, túi chỉ chứa được tối đa trọng lượng `W`. Chọn cách bỏ đồ vào túi sao cho tổng giá trị lớn nhất, không vượt quá trọng lượng `W`.

```cpp
int knapsack(vector<int> &trongLuong, vector<int> &giaTri, int n, int W) {
    vector<vector<int>> dp(n + 1, vector<int>(W + 1, 0));

    for (int i = 1; i <= n; i++) {
        for (int w = 0; w <= W; w++) {
            if (trongLuong[i-1] > w) {
                dp[i][w] = dp[i-1][w]; // món này quá nặng, không bỏ vào được
            } else {
                // Chọn giá trị lớn hơn giữa: không lấy món i, hoặc lấy món i
                dp[i][w] = max(dp[i-1][w], dp[i-1][w - trongLuong[i-1]] + giaTri[i-1]);
            }
        }
    }

    return dp[n][W];
}
```

**Giải thích:** `dp[i][w]` nghĩa là "giá trị lớn nhất có thể đạt được, nếu chỉ được chọn trong `i` món đồ đầu tiên, và túi giới hạn trọng lượng `w`". Với mỗi món đồ, ta có 2 lựa chọn: **không lấy** (giữ nguyên kết quả tốt nhất mà không cần món này) hoặc **lấy** (cộng thêm giá trị món này, trừ bớt trọng lượng còn trống) — DP giúp thử cả 2 lựa chọn một cách có hệ thống mà không cần liệt kê tất cả các tổ hợp có thể (sẽ là O(2ⁿ) nếu làm brute force).

**Ví dụ dễ hình dung:** giống việc bạn đi leo núi, ba lô chỉ chở được tối đa 10kg, nhưng có rất nhiều đồ muốn mang (lều 5kg giá trị cao, đồ ăn 3kg, quần áo 4kg...). Với mỗi món, bạn tự hỏi: "Nếu KHÔNG mang món này thì sao? Nếu CÓ mang thì sao, còn chỗ cho gì khác không?" — DP giúp bạn trả lời câu hỏi này cho tất cả các món một cách có tổ chức, tìm ra tổ hợp đồ mang theo tối ưu nhất.

## 6. Ví dụ thực tế: Chọn bài tập làm sao được nhiều điểm nhất trong thời gian giới hạn

Bạn có 1 buổi tối chỉ còn 180 phút để ôn thi, có nhiều bài tập khác nhau, mỗi bài tốn thời gian và cho một số điểm thưởng khác nhau — đây thực chất chính là bài toán Knapsack:

```cpp
#include <iostream>
#include <vector>
using namespace std;

int main() {
    int soBai;
    cout << "So bai tap: ";
    cin >> soBai;

    vector<int> thoiGian(soBai), diem(soBai);
    for (int i = 0; i < soBai; i++) {
        cout << "Bai " << (i+1) << " - thoi gian (phut) va diem thuong: ";
        cin >> thoiGian[i] >> diem[i];
    }

    int gioiHanThoiGian = 180;
    vector<vector<int>> dp(soBai + 1, vector<int>(gioiHanThoiGian + 1, 0));

    for (int i = 1; i <= soBai; i++) {
        for (int t = 0; t <= gioiHanThoiGian; t++) {
            if (thoiGian[i-1] > t) {
                dp[i][t] = dp[i-1][t];
            } else {
                dp[i][t] = max(dp[i-1][t], dp[i-1][t - thoiGian[i-1]] + diem[i-1]);
            }
        }
    }

    cout << "So diem thuong toi da co the dat duoc: " << dp[soBai][gioiHanThoiGian] << endl;
    return 0;
}
```

Đây là ứng dụng cực kỳ thực tế của Knapsack: quản lý thời gian ôn thi sao cho hiệu quả nhất, y hệt logic của bài toán "chọn đồ mang vào túi" nhưng đổi "trọng lượng" thành "thời gian" và "giá trị" thành "điểm thưởng".

## Bài tập gợi ý

1. Cài đặt DP tính số Fibonacci bằng cả 2 cách: memoization và tabulation, so sánh tốc độ chạy với `n = 45`.
2. Bài toán "Đường đi trong lưới": cho lưới ô vuông `m x n`, chỉ được đi xuống hoặc sang phải, đếm số đường đi khác nhau từ góc trên-trái tới góc dưới-phải (gợi ý: `dp[i][j] = dp[i-1][j] + dp[i][j-1]`).
3. Bài toán "Dãy con tăng dài nhất" (Longest Increasing Subsequence - LIS): cho 1 mảng số, tìm độ dài dãy con dài nhất mà các phần tử tăng dần (không nhất thiết liên tiếp).
4. Bài toán "Đổi tiền xu": cho các loại tiền xu mệnh giá khác nhau, tìm số lượng xu ít nhất để đổi đủ một số tiền `S` cho trước.
5. (Nâng cao nhẹ) Bài toán "Dãy con chung dài nhất" (Longest Common Subsequence - LCS) giữa 2 chuỗi — đây là bài toán DP 2 chiều kinh điển, thường xuất hiện trong các đề thi HSG cấp tỉnh/quốc gia. Tìm hiểu công thức truy hồi và thử tự cài đặt.
