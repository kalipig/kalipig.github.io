## Solution

本题的关键是计算所有 $k$ 元素集合的危险值平均值。根据题目特殊结构，任意 $k$ 元素集合 $S$ 有唯一相邻节点 $w$，危险值为 $S$ 中各节点到 $w$ 的边权和。

核心思路：\
每条边 $u,v$ 的贡献取决于其在多少个集合 $S$ 中被计入危险值。
对于边 $u,v$，贡献次数为 $C_{d_{u}-1}^{k-1}$ + $C_{d _{v}-1}^{k-1}$ ，其中 $d_{u}$ 是 $u$ 的度数，$C$ 是组合数。\
总集合数为 $C_{n}^{k}$，平均值为所有边的总贡献除以总集合数。

## Code

```cpp
#include <iostream>
#include <vector>
using namespace std;

typedef long long ll;

ll comb(int m, int t) {
    if (t == 0) return 1;
    if (m < t) return 0;
    ll res = 1;
    for (int i = 1; i <= t; ++i) {
        res = res * (m - t + i) / i;
    }
    return res;
}

int main() {
    int n, k;
    cin >> n >> k;
    vector<int> d(n + 1, 0); // 度数，1-based
    vector<tuple<int, int, int>> edges;

    for (int i = 1; i <= n - 1; ++i) {
        for (int j = i + 1; j <= n; ++j) {
            int c;
            cin >> c;
            if (c != -1) {
                edges.emplace_back(i, j, c);
                d[i]++;
                d[j]++;
            }
        }
    }

    int t = k - 1;
    ll sum = 0;
    for (auto& e : edges) {
        int u = get<0>(e), v = get<1>(e), c = get<2>(e);
        ll c1 = comb(d[u] - 1, t);
        ll c2 = comb(d[v] - 1, t);
        sum += c * (c1 + c2);
    }

    ll total = comb(n, k);
    cout << sum / total << endl;

    return 0;
}
```