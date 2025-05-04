## CF1679D

### Solution

看到最大值最小，马上想到二分答案。

那就主要看 ${check}$ 函数怎么写，当二分答案为 $x$ 时，这里为了方便，先输入时把所有边存下来，然后每次二分重新建图，保证只有端点点权小于 $x$ 的。

在保证最大值一定小于 $x$ 后，我们要走 $k$ 步，可以直接找最长路，看一下有没有 $k$ 就可以了。

那么这里采用拓扑排序来动态规划，$f_i$ 表示从 $i$ 这个点出发能走多远，那么转移特别简单：$f_v=\max(f_v,f_u+1)$。

最后枚举找到大于 $k$ 的 $f_i$ 就好了。

### Code

~~~cpp
#include <bits/stdc++.h>
using namespace std;
using LL = long long;
const LL N = 2e5 + 10;
vector<vector<LL>> g(N);
LL n, m, a[N],k,ind[N], f[N];
pair<LL, LL> E[N]; 
void addEdge(LL u, LL v) {
    g[u].push_back(v);
}
bool check(LL x) {
    fill(g.begin(), g.end(), vector<LL>());
    memset(ind, 0, sizeof(ind));
    memset(f, 0, sizeof(f));
    for (LL i = 1; i <= m; i++) {
        if (a[E[i].first] <= x && a[E[i].second] <= x) {
            addEdge(E[i].first, E[i].second);
            ind[E[i].second]++;
        }
    }
    queue<LL> q;
    for (LL i = 1; i <= n; i++) {
        if (!ind[i] && a[i] <= x) {
            q.push(i);
            f[i] = 1;
        }
    }
    while (!q.empty()) {
        LL u = q.front();
        q.pop();
        for (LL v : g[u]) {
            f[v] = max(f[v], f[u] + 1);
            if (--ind[v] == 0) {
                q.push(v);
            }
        }
    }
    for (LL i = 1; i <= n; i++) {
        if (ind[i] || f[i] >= k) {
            return true;
        }
    }
    return false;
}
int main() {
    ios::sync_with_stdio(false);cin.tie(nullptr);cout.tie(nullptr);
    cin >> n >> m >> k;
    for (LL i = 1; i <= n; i++) {
        cin >> a[i];
    }
    for (LL i = 1; i <= m; i++) {
        cin >> E[i].first >> E[i].second;
    }
    LL l = 1, r = 1000000001, mid, ans = -1;
    while (l < r) {
        mid = (l + r) >> 1;
        if (check(mid)) {
            ans = mid;
            r = mid;
        } else {
            l = mid + 1;
        }
    }
    cout << ans << endl;
    return 0;
}
~~~