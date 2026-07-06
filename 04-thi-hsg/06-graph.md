# Bài 6: Lý Thuyết Đồ Thị — BFS, DFS, Dijkstra

## 1. Đồ thị (Graph) là gì?

Đồ thị gồm các **đỉnh (node/vertex)** và các **cạnh (edge)** nối giữa chúng. Rất nhiều bài toán thực tế có thể mô hình hóa thành đồ thị: bản đồ giao thông (đỉnh = ngã tư, cạnh = con đường), mạng xã hội (đỉnh = người dùng, cạnh = quan hệ bạn bè), sơ đồ mạng máy tính...

## 2. Biểu diễn đồ thị bằng danh sách kề (Adjacency List)

```cpp
#include <vector>
using namespace std;

int n; // số đỉnh
vector<int> ke[100005]; // ke[u] chứa danh sách các đỉnh nối trực tiếp với u

// Thêm cạnh vô hướng giữa u và v
void themCanh(int u, int v) {
    ke[u].push_back(v);
    ke[v].push_back(u); // vì vô hướng nên thêm cả 2 chiều
}
```

**Ví dụ dễ hình dung:** đồ thị giống như **sơ đồ các tuyến xe buýt trong thành phố** — mỗi trạm xe buýt là 1 đỉnh, mỗi tuyến xe nối 2 trạm là 1 cạnh. Danh sách kề `ke[u]` chính là "từ trạm u, có thể đi thẳng tới những trạm nào".

## 3. Duyệt theo chiều sâu — DFS (Depth-First Search)

DFS đi càng sâu càng tốt theo 1 nhánh trước, hết đường mới quay lại (backtrack) thử nhánh khác.

```cpp
bool daTham[100005];

void dfs(int u) {
    daTham[u] = true;
    cout << "Tham dinh: " << u << endl;

    for (int v : ke[u]) {
        if (!daTham[v]) {
            dfs(v); // đi sâu vào nhánh này trước
        }
    }
}
```

**Ví dụ dễ hình dung:** DFS giống như đi lạc trong mê cung và áp dụng chiến thuật "luôn đi thẳng theo 1 lối cho tới khi gặp ngõ cụt, rồi mới quay lại điểm rẽ gần nhất để thử lối khác" — đi sâu tối đa vào 1 hướng trước khi thử hướng khác, thay vì thăm dò tất cả các lối gần trước.

## 4. Duyệt theo chiều rộng — BFS (Breadth-First Search)

BFS thăm hết các đỉnh gần nhất (kề trực tiếp) trước, rồi mới lan ra các đỉnh xa hơn — dùng `queue` để cài đặt.

```cpp
#include <queue>

void bfs(int start) {
    queue<int> q;
    bool daTham[100005] = {false};

    q.push(start);
    daTham[start] = true;

    while (!q.empty()) {
        int u = q.front();
        q.pop();
        cout << "Tham dinh: " << u << endl;

        for (int v : ke[u]) {
            if (!daTham[v]) {
                daTham[v] = true;
                q.push(v);
            }
        }
    }
}
```

**Ví dụ dễ hình dung:** BFS giống như **thả một giọt nước xuống ao và quan sát gợn sóng lan ra** — sóng lan đều theo từng vòng tròn đồng tâm mở rộng dần (thăm hết các đỉnh cách 1 bước trước, rồi mới tới các đỉnh cách 2 bước, v.v.), thay vì "phóng thẳng" theo 1 hướng như DFS.

**Ứng dụng quan trọng nhất của BFS:** tìm đường đi ngắn nhất (ít cạnh nhất) giữa 2 đỉnh trong đồ thị **không trọng số** — vì BFS luôn thăm các đỉnh theo đúng thứ tự "gần nhất trước", đỉnh nào được thăm sớm nhất chắc chắn là đỉnh gần nhất từ điểm xuất phát.

## 5. Đồ thị có trọng số & thuật toán Dijkstra (tìm đường đi ngắn nhất)

Khi mỗi cạnh có 1 "chi phí" (ví dụ khoảng cách km, thời gian di chuyển), BFS không còn đúng nữa (vì "ít cạnh nhất" chưa chắc là "chi phí nhỏ nhất"). Cần dùng thuật toán Dijkstra:

```cpp
#include <queue>
#include <vector>
#include <climits>
using namespace std;

vector<pair<int,int>> keTrongSo[100005]; // ke[u] = {(v1, trongso1), (v2, trongso2), ...}

vector<int> dijkstra(int start, int n) {
    vector<int> khoangCach(n + 1, INT_MAX);
    khoangCach[start] = 0;

    // priority_queue lưu {khoảng cách, đỉnh}, luôn lấy ra khoảng cách NHỎ NHẤT trước
    priority_queue<pair<int,int>, vector<pair<int,int>>, greater<pair<int,int>>> pq;
    pq.push({0, start});

    while (!pq.empty()) {
        auto [d, u] = pq.top();
        pq.pop();

        if (d > khoangCach[u]) continue; // đã có đường đi tốt hơn rồi, bỏ qua

        for (auto [v, trongso] : keTrongSo[u]) {
            if (khoangCach[u] + trongso < khoangCach[v]) {
                khoangCach[v] = khoangCach[u] + trongso;
                pq.push({khoangCach[v], v});
            }
        }
    }

    return khoangCach;
}
```

**Giải thích:** Dijkstra dùng `priority_queue` (đã học ở Phần 3) để **luôn xử lý đỉnh có khoảng cách ngắn nhất hiện biết trước** — giống chiến lược tham lam (Greedy) đã học ở bài trước, nhưng áp dụng vào bài toán đồ thị.

**Ví dụ dễ hình dung:** Dijkstra giống hệt cách app chỉ đường (Google Maps) tìm đường đi ngắn nhất giữa 2 địa điểm — nó không chỉ đếm "số ngã tư phải qua" (đó là BFS) mà tính toán "tổng thời gian/khoảng cách di chuyển thực tế", ưu tiên khám phá tuyến đường có tổng chi phí nhỏ nhất tính tới thời điểm hiện tại trước, rồi mới mở rộng ra các tuyến khác.

## 6. Ví dụ thực tế: Tìm đường đi ngắn nhất giữa các thành phố

```cpp
#include <iostream>
#include <vector>
#include <queue>
using namespace std;

int main() {
    int n, m; // n thành phố, m con đường
    cout << "So thanh pho va so con duong: ";
    cin >> n >> m;

    vector<pair<int,int>> ke[1005];

    cout << "Nhap tung con duong (thanh_pho_1 thanh_pho_2 khoang_cach):" << endl;
    for (int i = 0; i < m; i++) {
        int u, v, w;
        cin >> u >> v >> w;
        ke[u].push_back({v, w});
        ke[v].push_back({u, w}); // đường 2 chiều
    }

    int diemBatDau;
    cout << "Thanh pho xuat phat: ";
    cin >> diemBatDau;

    vector<int> khoangCach(n + 1, INT_MAX);
    khoangCach[diemBatDau] = 0;

    priority_queue<pair<int,int>, vector<pair<int,int>>, greater<pair<int,int>>> pq;
    pq.push({0, diemBatDau});

    while (!pq.empty()) {
        auto [d, u] = pq.top();
        pq.pop();
        if (d > khoangCach[u]) continue;

        for (auto [v, trongso] : ke[u]) {
            if (khoangCach[u] + trongso < khoangCach[v]) {
                khoangCach[v] = khoangCach[u] + trongso;
                pq.push({khoangCach[v], v});
            }
        }
    }

    cout << "\nKhoang cach ngan nhat tu thanh pho " << diemBatDau << " den cac noi:" << endl;
    for (int i = 1; i <= n; i++) {
        cout << "Thanh pho " << i << ": " << khoangCach[i] << " km" << endl;
    }

    return 0;
}
```

Đây chính là bài toán nền tảng của mọi hệ thống chỉ đường/định tuyến thực tế — từ Google Maps, ứng dụng đặt xe công nghệ, cho tới định tuyến gói tin trên internet, đều dựa trên nguyên lý của thuật toán Dijkstra (hoặc các biến thể tối ưu hơn của nó).

## Bài tập gợi ý

1. Cài đặt DFS và BFS cho 1 đồ thị đơn giản, so sánh thứ tự các đỉnh được thăm giữa 2 cách duyệt.
2. Dùng BFS đếm số thành phần liên thông trong 1 đồ thị (những nhóm đỉnh nối được với nhau, nhưng không nối được sang nhóm khác).
3. Dùng BFS tìm đường đi ngắn nhất (số cạnh ít nhất) giữa 2 đỉnh trong đồ thị không trọng số, in ra cả đường đi cụ thể (không chỉ độ dài).
4. Cài đặt Dijkstra hoàn chỉnh và thử với 1 đồ thị có trọng số ít nhất 6 đỉnh, kiểm tra kết quả bằng tay.
5. (Nâng cao nhẹ) Tìm hiểu thuật toán tìm Cây khung nhỏ nhất (Minimum Spanning Tree - MST) như Kruskal hoặc Prim — đây là bài toán khác dùng Greedy trên đồ thị, thường xuất hiện trong đề thi HSG (ví dụ: bài toán nối các thành phố bằng cáp quang với tổng chi phí nhỏ nhất).
