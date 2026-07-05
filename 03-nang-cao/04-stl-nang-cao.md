# Bài 4: STL Nâng Cao — set, map, stack, queue, priority_queue

## 1. `set` — tập hợp không trùng lặp, tự động sắp xếp

```cpp
#include <set>
using namespace std;

set<int> s;
s.insert(5);
s.insert(2);
s.insert(8);
s.insert(2); // trùng, sẽ không được thêm lần nữa

for (int x : s) cout << x << " "; // in ra: 2 5 8 (tự động sắp xếp tăng dần!)

cout << s.count(5) << endl; // 1 nếu có, 0 nếu không có
s.erase(5); // xóa phần tử 5
```

**Ví dụ dễ hình dung:** `set` giống như một **danh sách khách mời VIP** — mỗi người chỉ được ghi tên 1 lần dù bạn cố ghi thêm lần nữa (tự động loại trùng), và danh sách luôn được ban tổ chức sắp xếp gọn gàng theo thứ tự nào đó (ở đây là tăng dần) để dễ tra cứu.

## 2. `map` — lưu trữ theo cặp khóa-giá trị, tự động sắp xếp theo khóa

```cpp
#include <map>
using namespace std;

map<string, int> diem; // khóa là tên (string), giá trị là điểm (int)

diem["Khoi"] = 8;
diem["Lan"] = 9;
diem["Nam"] = 7;

cout << diem["Khoi"] << endl; // 8 -> tra cứu như tra từ điển

for (auto p : diem) {
    cout << p.first << ": " << p.second << endl; // duyệt qua từng cặp khóa-giá trị
}

if (diem.count("Khoi") > 0) {
    cout << "Co ton tai Khoi trong map" << endl;
}
```

**Giải thích:** `map` hoạt động giống hệt 1 cuốn từ điển thật — bạn tra theo "từ khóa" (`diem["Khoi"]`) để lấy ra "định nghĩa" tương ứng (điểm số 8), thay vì phải nhớ vị trí số thứ tự như mảng thông thường.

**Ví dụ dễ hình dung:** `map` giống như **danh bạ điện thoại** — bạn tra tên người (khóa) để lấy ra số điện thoại (giá trị), không cần nhớ "số điện thoại của Khôi nằm ở dòng thứ mấy trong sổ".

## 3. `stack` — ngăn xếp, vào sau ra trước (LIFO)

```cpp
#include <stack>
using namespace std;

stack<int> st;
st.push(1);
st.push(2);
st.push(3);

cout << st.top() << endl; // 3 - xem phần tử trên cùng (mới thêm gần nhất)
st.pop();                  // bỏ phần tử trên cùng -> còn {1, 2}
cout << st.top() << endl; // 2
```

**Ví dụ dễ hình dung:** `stack` giống hệt **chồng đĩa trong bếp** — bạn chỉ có thể đặt thêm đĩa lên trên cùng (`push`) và chỉ có thể lấy đĩa từ trên cùng xuống (`pop`), không thể rút đĩa ở giữa hay dưới đáy chồng ra mà không làm đổ cả chồng. Đĩa đặt vào sau cùng lại là đĩa được lấy ra đầu tiên — đúng tinh thần "vào sau, ra trước" (LIFO - Last In First Out).

## 4. `queue` — hàng đợi, vào trước ra trước (FIFO)

```cpp
#include <queue>
using namespace std;

queue<int> q;
q.push(1);
q.push(2);
q.push(3);

cout << q.front() << endl; // 1 - xem phần tử đầu hàng (vào trước nhất)
q.pop();                    // bỏ phần tử đầu hàng -> còn {2, 3}
cout << q.front() << endl; // 2
```

**Ví dụ dễ hình dung:** `queue` giống hệt **xếp hàng mua vé xem phim** — ai xếp hàng trước thì được phục vụ trước (`front`), người mới tới phải đứng vào cuối hàng (`push`). Đây là tinh thần "vào trước, ra trước" (FIFO - First In First Out) — ngược hoàn toàn với `stack`.

## 5. `priority_queue` — hàng đợi ưu tiên, luôn lấy ra phần tử lớn nhất trước

```cpp
#include <queue>
using namespace std;

priority_queue<int> pq;
pq.push(5);
pq.push(1);
pq.push(9);
pq.push(3);

cout << pq.top() << endl; // 9 - luôn là giá trị LỚN NHẤT hiện có, bất kể thứ tự thêm vào
pq.pop();
cout << pq.top() << endl; // 5 - lớn nhất trong số còn lại

// Muốn lấy ra nhỏ nhất trước, dùng:
priority_queue<int, vector<int>, greater<int>> pqMin;
```

**Ví dụ dễ hình dung:** `priority_queue` giống hệt **phòng cấp cứu bệnh viện** — không phải ai tới trước được khám trước (khác với `queue` thông thường ở phòng khám dịch vụ), mà bệnh nhân **nặng nhất** luôn được ưu tiên khám trước, bất kể họ mới tới hay đã chờ lâu.

## 6. Bảng so sánh nhanh khi nào dùng cái nào

| Cấu trúc | Đặc điểm | Ví dụ thực tế tương ứng |
|---|---|---|
| `set` | không trùng, tự sắp xếp | Danh sách khách mời VIP |
| `map` | tra cứu theo khóa | Danh bạ điện thoại |
| `stack` | LIFO — vào sau ra trước | Chồng đĩa trong bếp |
| `queue` | FIFO — vào trước ra trước | Xếp hàng mua vé |
| `priority_queue` | luôn lấy giá trị ưu tiên nhất | Phòng cấp cứu bệnh viện |

## 7. Ví dụ thực tế: Mô phỏng quầy phục vụ khách theo mức độ khẩn cấp

```cpp
#include <iostream>
#include <queue>
using namespace std;

struct BenhNhan {
    string ten;
    int mucDoNang; // số càng lớn càng cần khám gấp

    bool operator<(const BenhNhan &khac) const {
        return mucDoNang < khac.mucDoNang; // để priority_queue biết cách so sánh
    }
};

int main() {
    priority_queue<BenhNhan> phongCapCuu;

    phongCapCuu.push({"Anh A", 3});
    phongCapCuu.push({"Chi B", 8}); // nặng hơn, sẽ được khám trước dù tới sau
    phongCapCuu.push({"Anh C", 5});

    cout << "Thu tu kham benh:" << endl;
    while (!phongCapCuu.empty()) {
        BenhNhan bn = phongCapCuu.top();
        phongCapCuu.pop();
        cout << "- " << bn.ten << " (muc do: " << bn.mucDoNang << ")" << endl;
    }
    // Kết quả: Chi B (8) -> Anh C (5) -> Anh A (3)

    return 0;
}
```

Đây là ứng dụng thực tế điển hình của `priority_queue`: dù "Chị B" đến sau "Anh A", nhưng vì mức độ nghiêm trọng cao hơn nên được xử lý trước — logic này được dùng rất nhiều trong hệ thống thực tế (lập lịch CPU, xử lý đơn hàng ưu tiên, định tuyến mạng...).

## Bài tập gợi ý

1. Dùng `set<int>` để loại bỏ các phần tử trùng lặp trong 1 danh sách số, sau đó in ra danh sách đã sắp xếp không trùng.
2. Dùng `map<string, int>` đếm số lần xuất hiện của mỗi từ trong 1 câu (tách câu thành từng từ, đếm bằng map).
3. Dùng `stack<char>` kiểm tra một chuỗi ngoặc đơn `()` có hợp lệ (đóng-mở đúng cặp) hay không.
4. Mô phỏng hàng đợi khách hàng tại quầy thu ngân bằng `queue<string>`, cho phép thêm khách vào cuối hàng và phục vụ khách đầu hàng.
5. (Nâng cao nhẹ) Dùng `priority_queue` để giải bài toán: cho `n` công việc, mỗi công việc có thời gian hoàn thành, tìm thứ tự thực hiện công việc sao cho tổng thời gian chờ của tất cả công việc là nhỏ nhất (gợi ý: luôn ưu tiên làm công việc ngắn nhất trước).
