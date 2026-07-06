# Bài 2: Kỹ Thuật Duyệt — Brute Force, Two Pointers, Sliding Window

## 1. Brute Force (vét cạn) — thử mọi khả năng

Cách đơn giản nhất: thử tất cả các trường hợp có thể, kiểm tra từng cái xem có thỏa mãn yêu cầu không.

```cpp
// Tìm cặp số (i, j) trong mảng có tổng bằng x
pair<int,int> timCapTong(vector<int> &a, int x) {
    int n = a.size();
    for (int i = 0; i < n; i++) {
        for (int j = i + 1; j < n; j++) {
            if (a[i] + a[j] == x) return {i, j};
        }
    }
    return {-1, -1}; // không tìm thấy
}
```

**Độ phức tạp:** O(n²) — chấp nhận được nếu `n` nhỏ (vài nghìn), nhưng sẽ chậm nếu `n` lớn (trăm nghìn trở lên). Đây là lúc cần tới các kỹ thuật tối ưu hơn như Two Pointers dưới đây.

## 2. Two Pointers (hai con trỏ) — tối ưu bài toán trên mảng đã sắp xếp

Ý tưởng: dùng 2 chỉ số (con trỏ) di chuyển từ 2 đầu mảng vào giữa, thay vì lồng 2 vòng lặp.

```cpp
// Tìm cặp số có tổng bằng x, trong mảng ĐÃ SẮP XẾP - O(n) thay vì O(n²)
pair<int,int> timCapTongToiUu(vector<int> &a, int x) {
    int trai = 0, phai = a.size() - 1;

    while (trai < phai) {
        int tong = a[trai] + a[phai];
        if (tong == x) return {trai, phai};
        else if (tong < x) trai++;  // tổng còn nhỏ, cần tăng -> dịch trái sang phải
        else phai--;                 // tổng quá lớn, cần giảm -> dịch phải sang trái
    }
    return {-1, -1};
}
```

**Giải thích:** vì mảng đã sắp xếp tăng dần, nếu tổng 2 đầu quá nhỏ, ta biết chắc phải tăng giá trị lên bằng cách dịch con trỏ trái sang phải (lấy số lớn hơn); nếu tổng quá lớn, dịch con trỏ phải sang trái (lấy số nhỏ hơn). Nhờ tính chất "đã sắp xếp", ta không cần thử lại toàn bộ cặp mà chỉ cần duyệt qua mảng đúng 1 lượt.

**Ví dụ dễ hình dung:** giống trò chơi "đoán tổng 2 số" với 1 dãy ghế được xếp theo chiều cao tăng dần từ trái sang phải trong 1 hàng dài. Bạn đặt 1 tay ở đầu hàng thấp nhất, 1 tay ở cuối hàng cao nhất. Nếu tổng chiều cao 2 người đang chọn còn thấp hơn mục tiêu, bạn nhích tay trái sang phải (chọn người cao hơn). Nếu tổng cao hơn mục tiêu, bạn nhích tay phải sang trái (chọn người thấp hơn). Cứ thế cho tới khi 2 tay gặp đúng cặp mong muốn hoặc gặp nhau ở giữa.

## 3. Sliding Window (cửa sổ trượt) — tối ưu bài toán về đoạn con liên tiếp

Dùng khi cần tìm đoạn con liên tiếp (subarray) thỏa mãn điều kiện nào đó, ví dụ "tổng lớn nhất của đoạn con có đúng k phần tử".

```cpp
// Tìm tổng lớn nhất của đoạn con liên tiếp có đúng k phần tử
int tongLonNhatDoanK(vector<int> &a, int k) {
    int n = a.size();
    int tongHienTai = 0;

    for (int i = 0; i < k; i++) tongHienTai += a[i]; // tính tổng k phần tử đầu tiên

    int tongMax = tongHienTai;

    for (int i = k; i < n; i++) {
        tongHienTai += a[i] - a[i - k]; // "trượt cửa sổ": thêm phần tử mới, bỏ phần tử cũ nhất
        tongMax = max(tongMax, tongHienTai);
    }

    return tongMax;
}
```

**Giải thích:** thay vì tính lại tổng từ đầu mỗi lần dịch chuyển đoạn (sẽ tốn O(n×k)), ta chỉ cần "trượt" cửa sổ đi 1 bước: cộng thêm phần tử mới vào bên phải, trừ đi phần tử cũ nhất bị đẩy ra khỏi bên trái — độ phức tạp giảm xuống chỉ còn O(n).

**Ví dụ dễ hình dung:** giống như bạn đang nhìn qua 1 khung cửa sổ nhỏ có kích thước cố định (`k` ô) trên 1 dải băng dài các con số, và bạn di chuyển khung cửa sổ này từ trái sang phải trên dải băng. Mỗi lần dịch khung sang phải 1 ô, có đúng 1 số "lọt ra ngoài" khung ở bên trái và 1 số "lọt vào" khung ở bên phải — bạn chỉ cần cập nhật đúng 2 số đó thay vì đếm lại toàn bộ số đang nằm trong khung.

## 4. Khi nào dùng kỹ thuật nào?

| Tình huống | Kỹ thuật phù hợp |
|---|---|
| Bài toán nhỏ, `n` không quá lớn, muốn chắc chắn đúng trước đã | Brute Force |
| Mảng đã (hoặc có thể) sắp xếp, tìm cặp/bộ giá trị thỏa điều kiện | Two Pointers |
| Cần tìm đoạn con liên tiếp thỏa điều kiện (tổng, độ dài...) | Sliding Window |

## 5. Ví dụ thực tế: Tìm chuỗi ngày liên tiếp chi tiêu nhiều nhất trong tháng

Bạn ghi lại số tiền chi tiêu mỗi ngày trong tháng, muốn biết trong 7 ngày liên tiếp bất kỳ, khoảng nào bạn tiêu nhiều tiền nhất (để xem lại có phải tuần đó đi du lịch hay có sự kiện gì không):

```cpp
#include <iostream>
#include <vector>
using namespace std;

int main() {
    int soNgay;
    cout << "So ngay trong thang: ";
    cin >> soNgay;

    vector<int> chiTieu(soNgay);
    for (int i = 0; i < soNgay; i++) cin >> chiTieu[i];

    int k = 7; // xét mỗi khoảng 7 ngày liên tiếp
    int tongHienTai = 0;
    for (int i = 0; i < k; i++) tongHienTai += chiTieu[i];

    int tongMax = tongHienTai;
    int ngayBatDauMax = 0;

    for (int i = k; i < soNgay; i++) {
        tongHienTai += chiTieu[i] - chiTieu[i - k]; // trượt cửa sổ
        if (tongHienTai > tongMax) {
            tongMax = tongHienTai;
            ngayBatDauMax = i - k + 1;
        }
    }

    cout << "Tuan chi tieu nhieu nhat bat dau tu ngay " << (ngayBatDauMax + 1)
         << ", tong chi: " << tongMax << " dong" << endl;

    return 0;
}
```

Đây chính là kỹ thuật Sliding Window được áp dụng vào bài toán quản lý chi tiêu cá nhân rất thực tế — nhiều app quản lý tài chính dùng đúng thuật toán này để cảnh báo "tuần chi tiêu bất thường".

## Bài tập gợi ý

1. Dùng Brute Force viết chương trình tìm 3 số trong mảng có tổng bằng `x` cho trước (bài toán "3Sum").
2. Dùng Two Pointers viết chương trình kiểm tra 1 chuỗi có phải Palindrome hay không (đã học ở Phần 1, giờ hãy nhận diện đó chính là kỹ thuật Two Pointers).
3. Dùng Sliding Window tìm đoạn con liên tiếp dài nhất trong mảng chỉ gồm số 0 và 1, sao cho đoạn đó có nhiều nhất `k` số 0 (có thể "đổi" tối đa k số 0 thành 1).
4. Cho mảng số dương, dùng Two Pointers tìm xem có tồn tại 2 số có tích bằng giá trị `x` cho trước hay không (mảng đã sắp xếp tăng dần).
5. (Nâng cao nhẹ) Tìm đoạn con liên tiếp NGẮN NHẤT có tổng lớn hơn hoặc bằng `x` cho trước (đây là biến thể Sliding Window với kích thước cửa sổ thay đổi linh hoạt, không cố định `k` như ví dụ trên — gợi ý: dùng 2 con trỏ trái/phải, mở rộng khi tổng chưa đủ, thu hẹp khi tổng đã đủ).
