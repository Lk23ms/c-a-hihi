# Bài 5: Thuật Toán Tham Lam (Greedy)

## 1. Ý tưởng cốt lõi

Greedy (tham lam) là chiến lược: **ở mỗi bước, chọn phương án tốt nhất ngay tại thời điểm đó**, mà không cần suy nghĩ lâu dài về ảnh hưởng tới các bước sau. Khác với DP (xét đầy đủ mọi khả năng), Greedy đưa ra quyết định "một đi không trở lại" — nhanh hơn nhiều nhưng **không phải lúc nào cũng cho ra đáp án đúng**.

**Ví dụ dễ hình dung:** giống như đi siêu thị mua đồ giảm giá — chiến lược tham lam là "thấy món nào giảm giá nhiều nhất thì mua trước", không cần tính toán phức tạp xem tổ hợp mua hàng nào tối ưu nhất cho cả giỏ hàng. Cách này nhanh và thường ổn với việc mua sắm hàng ngày, nhưng chưa chắc luôn là lựa chọn tối ưu tuyệt đối.

## 2. Ví dụ kinh điển: Bài toán trả tiền thối với ít tờ tiền nhất

```cpp
#include <iostream>
#include <vector>
using namespace std;

int demSoToTien(int soTien, vector<int> menhGia) {
    int soTo = 0;

    for (int i = 0; i < menhGia.size(); i++) {  // menhGia đã sắp xếp giảm dần
        soTo += soTien / menhGia[i];  // lấy càng nhiều tờ mệnh giá lớn càng tốt
        soTien %= menhGia[i];
    }

    return soTo;
}

int main() {
    vector<int> menhGia = {500000, 200000, 100000, 50000, 20000, 10000};
    int soTien = 370000;

    cout << "So to tien it nhat: " << demSoToTien(soTien, menhGia) << endl;
    // 1 to 200000 + 1 to 100000 + 1 to 50000 + 1 to 20000 = 4 tờ
    return 0;
}
```

**Giải thích:** với mỗi mệnh giá tiền (từ lớn tới nhỏ), luôn lấy càng nhiều tờ mệnh giá đó càng tốt trước khi chuyển sang mệnh giá nhỏ hơn — đây chính là chiến lược "tham lam nhất tại từng bước".

⚠️ **Lưu ý quan trọng:** thuật toán tham lam này **chỉ đúng với hệ mệnh giá tiền Việt Nam** (và nhiều nước khác có hệ mệnh giá được thiết kế "đẹp"). Với hệ mệnh giá bất kỳ (ví dụ chỉ có tờ 1, 3, 4 để đổi số tiền 6 — tham lam sẽ chọn 4+1+1 = 3 tờ, nhưng đáp án tối ưu thực ra là 3+3 = 2 tờ!), Greedy có thể cho ra đáp án SAI. Đây là lý do bài toán đổi tiền tổng quát phải giải bằng DP (đã học ở bài trước), không phải lúc nào Greedy cũng dùng được.

## 3. Ví dụ: Bài toán chọn hoạt động không trùng giờ (Activity Selection)

Cho danh sách các công việc, mỗi công việc có giờ bắt đầu và giờ kết thúc, chọn ra tối đa bao nhiêu công việc có thể làm mà không bị trùng giờ (làm xong việc này mới sang việc khác).

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

struct CongViec {
    int batDau, ketThuc;
};

bool soSanh(CongViec a, CongViec b) {
    return a.ketThuc < b.ketThuc; // ưu tiên việc kết thúc SỚM nhất trước
}

int chonToiDaCongViec(vector<CongViec> &ds) {
    sort(ds.begin(), ds.end(), soSanh);

    int soLuong = 1;
    int gioKetThucGanNhat = ds[0].ketThuc;

    for (int i = 1; i < ds.size(); i++) {
        if (ds[i].batDau >= gioKetThucGanNhat) { // không trùng giờ với việc vừa chọn
            soLuong++;
            gioKetThucGanNhat = ds[i].ketThuc;
        }
    }

    return soLuong;
}
```

**Giải thích:** ý tưởng tham lam ở đây là **luôn ưu tiên chọn công việc kết thúc sớm nhất** trong các công việc còn khả thi — vì việc kết thúc càng sớm sẽ càng "chừa nhiều thời gian trống" cho các công việc tiếp theo, tối đa hóa số lượng công việc có thể làm. Đây là 1 trong số ít bài toán mà Greedy **được chứng minh luôn cho đáp án đúng**.

**Ví dụ dễ hình dung:** giống như bạn có 1 buổi chiều rảnh và nhận được nhiều lời mời đi chơi (mỗi lời mời có giờ bắt đầu/kết thúc riêng, không thể đi 2 nơi cùng lúc). Chiến lược thông minh là ưu tiên nhận lời mời nào **kết thúc sớm nhất trước**, để còn kịp giờ nhận thêm lời mời khác sau đó — tối đa hóa số buổi hẹn bạn tham gia được trong ngày.

## 4. Khi nào Greedy đúng, khi nào không?

Không có công thức chắc chắn 100%, nhưng vài dấu hiệu thường gặp:
- **Greedy hay đúng khi:** bài toán có tính chất "chọn tối ưu cục bộ dẫn đến tối ưu toàn cục" (cần được chứng minh, không phải đoán mò), ví dụ: chọn hoạt động không trùng giờ, cây khung nhỏ nhất (MST — sẽ gặp ở bài Graph).
- **Greedy hay sai khi:** bài toán yêu cầu cân nhắc nhiều lựa chọn có ảnh hưởng qua lại phức tạp lẫn nhau, ví dụ: bài toán cái túi 0/1 (Knapsack) — chọn món giá trị/kg cao nhất trước **không đảm bảo** tổng giá trị cuối cùng là tối ưu.

> **Lời khuyên khi thi HSG:** nếu không chắc chắn Greedy có đúng không, hãy thử tìm phản ví dụ (test case mà Greedy cho sai) trước khi nộp bài. Nếu không tìm được phản ví dụ sau khi thử kỹ, khả năng cao Greedy đúng cho bài đó.

## 5. Ví dụ thực tế: Sắp lịch xem phim tối đa trong ngày ở rạp chỉ có 1 phòng chiếu

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

struct SuatChieu {
    string tenPhim;
    int gioBatDau, gioKetThuc;
};

int main() {
    vector<SuatChieu> danhSach = {
        {"Phim A", 9, 11},
        {"Phim B", 10, 12},
        {"Phim C", 11, 13},
        {"Phim D", 13, 15},
        {"Phim E", 14, 16}
    };

    sort(danhSach.begin(), danhSach.end(), [](SuatChieu a, SuatChieu b) {
        return a.gioKetThuc < b.gioKetThuc;
    });

    cout << "Lich chieu toi uu (khong trung gio):" << endl;
    int gioKetThucGanNhat = -1;
    int demSoPhim = 0;

    for (auto &suat : danhSach) {
        if (suat.gioBatDau >= gioKetThucGanNhat) {
            cout << "- " << suat.tenPhim << " (" << suat.gioBatDau
                 << "h - " << suat.gioKetThuc << "h)" << endl;
            gioKetThucGanNhat = suat.gioKetThuc;
            demSoPhim++;
        }
    }

    cout << "Tong so phim chieu duoc trong ngay: " << demSoPhim << endl;
    return 0;
}
```

Đây chính xác là bài toán các rạp chiếu phim thực tế phải giải mỗi ngày khi chỉ có giới hạn số phòng chiếu — sắp lịch sao cho tối đa hóa số suất chiếu mà không bị trùng giờ.

## Bài tập gợi ý

1. Cài đặt lại bài toán trả tiền thối với hệ mệnh giá Việt Nam, thử nhiều số tiền khác nhau để kiểm chứng.
2. Chứng minh (bằng ví dụ cụ thể) rằng Greedy KHÔNG hoạt động đúng cho bài toán Knapsack 0/1 nếu chọn theo "giá trị/kg cao nhất trước".
3. Cài đặt bài toán chọn hoạt động không trùng giờ với ít nhất 8 công việc có giờ giấc chồng chéo phức tạp, kiểm tra kết quả bằng tay.
4. Bài toán "Số lượng ít nhất mũi tên bắn vỡ tất cả bong bóng" — cho các bong bóng dạng đoạn thẳng `[bắt đầu, kết thúc]` trên 1 trục số, tìm số mũi tên bắn theo chiều dọc ít nhất để mỗi bong bóng bị ít nhất 1 mũi tên xuyên qua (gợi ý: tương tự bài chọn hoạt động).
5. (Nâng cao nhẹ) Tìm hiểu thuật toán mã hóa Huffman (Huffman Coding) — một ứng dụng nổi tiếng của Greedy trong nén dữ liệu, dùng `priority_queue` (đã học ở Phần 3) để luôn ghép 2 phần tử có tần suất thấp nhất lại với nhau.
