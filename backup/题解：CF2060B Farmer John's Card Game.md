## 题解：CF2060B Farmer John's Card Game
因为要让牌堆从小到大，所以我们可以用结构体储存每头奶牛的牌和编号然后排序就可以了。\
又因为每一轮的出牌顺序一定，所以只要对第一轮（即每头奶牛的最小的牌）排序就可以得到答案。\
再枚举每一局，若后面的不符合这个顺序，就输出 $-1$。
```cpp
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

void solve() {
    int t;
    cin >> t;

    while (t--) {
        int n, m;
        cin >> n >> m;

        vector<vector<int>> c(n);
        vector<int> s(n), o(n);
		for(int i=0;i<n;i++) o[i]=i+1;
        for (int i = 0; i < n; ++i) {
            c[i].resize(m);
            for (int j = 0; j < m; ++j) {
                cin >> c[i][j];
            }
            sort(c[i].begin(), c[i].end());
            s[i] = c[i][0];
        }

        vector<pair<int, int>> z(n);
        for (int i = 0; i < n; ++i) {
            z[i] = {s[i], o[i]};
        }
        sort(z.begin(), z.end());

        for (int i = 0; i < n; ++i) {
            s[i] = z[i].first;
            o[i] = z[i].second;
        }

        int p = -1;
        bool v = true;

        for (int i = 0; i < m && v; ++i) {
            for (int j = 0; j < n; ++j) {
                int cw = o[j] - 1;
                bool pl = false;
                for (int k = 0; k < m; ++k) {
                    if (c[cw][k] > p) {
                        p = c[cw][k];
                        c[cw][k] = -1;
                        pl = true;
                        break;
                    }
                }
                if (!pl) {
                    v = false;
                    break;
                }
            }
        }

        if (v) {
            for (int i = 0; i < n; ++i) {
                cout << o[i] << " ";
            }
            cout << endl;
        } else {
            cout << -1 << endl;
        }
    }
}

int main() {
    solve();
    return 0;
}

```