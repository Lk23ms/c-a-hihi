# Bài 1: Độ Phức Tạp Thuật Toán (Big O)

## 1. Vì sao cần quan tâm tới độ phức tạp?

Một bài toán có thể giải bằng nhiều cách khác nhau, nhưng có cách nhanh, có cách chậm. Trong thi HSG, đề bài luôn giới hạn **thời gian chạy** (thường 1 giây) — nếu thuật toán quá chậm, dù code đúng logic vẫn bị chấm sai (gọi là lỗi TLE — Time Limit Exceeded).

**Ví dụ dễ hình dung:** giống như bạn cần tìm 1 cuốn sách trong thư viện. Nếu thư viện chỉ có 10 cuốn, bạn lật từng cuốn cũng chỉ mất vài giây. Nhưng nếu thư viện có 10 triệu cuốn, việc "lật từng cuốn" (thuật toán chậm) sẽ mất hàng giờ, trong khi biết cách tra theo mã số sách được sắp xếp sẵn (thuật toán nhanh) chỉ mất vài giây.

## 2. Ký hiệu Big O — đo tốc độ tăng của thời gian chạy

Big O không đo "chạy mất bao nhiêu giây chính xác" (vì còn phụ thuộc máy tính), mà đo **thời gian chạy tăng nhanh cỡ nào khi dữ liệu đầu vào `n` tăng lên**.

| Ký hiệu | Tên gọi | Ví dụ thuật toán | Với n = 1 triệu, ước lượng số bước |
|---|---|---|---|
| O(1) | hằng số | Truy cập a[5] | 1 |
| O(log n) | logarit | Tìm nhị phân | ~20 |
| O(n) | tuyến tính | Duyệt mảng 1 lượt | 1,000,000 |
| O(n log n) | n-log-n | Sắp xếp (sort) | ~20,000,000 |
| O(n²) | bình phương | Vòng lặp lồng 2 tầng (Bubble Sort) | 1,000,000,000,000 (rất chậm!) |
| O(2ⁿ) | mũ | Đệ quy không tối ưu (Fibonacci thô) | cực kỳ chậm, không khả thi |

**Mẹo nhớ nhanh cho thi HSG:** máy tính hiện đại làm được khoảng **100 triệu đến 1 tỷ phép tính mỗi giây**. Vậy nên nhìn vào giới hạn `n` của đề bài, bạn có thể đoán được thuật toán cần độ phức tạp cỡ nào:

| Giới hạn n trong đề | Độ phức tạp cần đạt |
|---|---|
| n ≤ 10 | O(n!) hoặc thử mọi hoán vị cũng được |
| n ≤ 20 | O(2ⁿ) |
| n ≤ 1,000 | O(n²) hoặc O(n³) |
| n ≤ 100,000 | O(n log n) |
| n ≤ 10,000,000 | O(n) |
| n cực lớn (10⁹ trở lên) | O(log n) hoặc O(1) |

## 3. Cách tính nhanh độ phức tạp qua code

```cpp
// O(1) - không phụ thuộc n
int a = arr[0];

// O(n) - 1 vòng lặp chạy n lần
for (int i = 0; i < n; i++) {
    // ...
}

// O(n²) - vòng lặp lồng nhau, mỗi tầng chạy n lần
for (int i = 0; i < n; i++) {
    for (int j = 0; j < n; j++) {
        // ...
    }
}

// O(log n) - mỗi bước giảm một nửa phạm vi (giống tìm nhị phân)
while (n > 1) {
    n = n / 2;
}
```

**Ví dụ dễ hình dung cho O(log n):** giống trò chơi "đoán số từ 1-100, mỗi lần đoán được báo cao hơn/thấp hơn" — nếu đoán thông minh (luôn đoán số ở giữa khoảng còn lại), bạn chỉ cần tối đa 7 lần đoán là chắc chắn tìm ra, dù khoảng số có tăng lên 1000 hay 1 triệu thì số lần đoán cũng chỉ tăng thêm rất ít — đó là đặc trưng của O(log n): tăng "rất chậm" dù n tăng "rất nhanh".

## 4. Ví dụ thực tế: So sánh 2 cách tìm bạn trùng tên trong danh sách lớp

**Cách 1 — O(n²): so sánh từng cặp học sinh**
```cpp
bool coTenTrung(vector<string> &ten, int n) {
    for (int i = 0; i < n; i++) {
        for (int j = i + 1; j < n; j++) {
            if (ten[i] == ten[j]) return true;
        }
    }
    return false;
}
```

**Cách 2 — O(n): dùng `set` để kiểm tra đã gặp tên đó chưa**
```cpp
bool coTenTrung(vector<string> &ten, int n) {
    set<string> daGap;
    for (int i = 0; i < n; i++) {
        if (daGap.count(ten[i])) return true; // đã gặp rồi -> trùng!
        daGap.insert(ten[i]);
    }
    return false;
}
```

Với lớp 40 học sinh, cả 2 cách đều nhanh như nhau (mắt thường không thấy khác biệt). Nhưng nếu đây là danh sách 1 triệu người (ví dụ tìm trùng số CMND toàn quốc), cách 1 (O(n²)) sẽ mất hàng giờ, trong khi cách 2 (O(n)) chỉ mất chưa tới 1 giây — đây chính là lý do vì sao hiểu độ phức tạp cực kỳ quan trọng khi dữ liệu thực tế lớn dần lên.

## Bài tập gợi ý

1. Xác định độ phức tạp của đoạn code: 1 vòng lặp `for` chạy từ 1 đến `n`, bên trong có 1 vòng lặp `for` khác chạy từ 1 đến 10 (không phụ thuộc n).
2. Cho thuật toán Bubble Sort đã học ở Phần 2, xác định độ phức tạp của nó và giải thích vì sao.
3. Nhìn vào giới hạn `n ≤ 100,000` của một đề bài, bạn nên tránh dùng độ phức tạp nào để không bị TLE?
4. So sánh độ phức tạp giữa tìm kiếm tuyến tính O(n) và tìm kiếm nhị phân O(log n) khi `n = 1,000,000` — ước lượng số bước chênh lệch giữa 2 cách.
5. (Nâng cao nhẹ) Giải thích vì sao thuật toán Fibonacci đệ quy thô (đã học ở Phần 2) có độ phức tạp O(2ⁿ), và vì sao con số này "bùng nổ" nhanh khủng khiếp khi `n` chỉ mới tăng lên 30-40.
