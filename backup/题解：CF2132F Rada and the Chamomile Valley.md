## CF2132F
### Solution
首先要求出 $1\rarr n$ 的所有路径必定经过的边，枚举每个路径肯定不行，但是从 $1$ 到 $n$ 一定是从 $1$ 的强连通分量到 $n$ 的强连通分量的边，那么关键边一定就是连接强连通分量的边。\
所以我们先缩点，然后再我们建新图时，记得记录新边原来的编号，这样方便找。
```cpp
for (int i = 1; i <= m; i++) {
        LL x = e[i].first, y = e[i].second;
        if (bel[x] != bel[y]) {
            v[bel[x]].emplace_back(bel[y], i);//记录原来的编号 i
            v[bel[y]].emplace_back(bel[x], i);
        }
    }
```
接下来用 ${dfs}$ 从包含 $1$ 的强连通分量里面出发到包含 $n$ 的强连通分量，并记录路径，这里记录的就是上面代码里的那个编号。\
然后我们就可以跑最短路，这里我们记录两个关键字，第一个是距离 $dis$，第二个是边的编号 $id$。\
对于任何关键边的端点，我们都加入优先队列里。\
在松弛时，如果 $dis_x=dis_{to}+1$，这里加一是因为我们计算的是一条边两边的端点，而边权为 $1$，我们就比较 $ans_{id_x}$ 和 $ans_{id_{to}}$。这里 $ans$ 是我们记录的答案，$to$ 是当前点，$x$ 是枚举的点。

### Code
```cpp
#include <bits/stdc++.h>
using LL = long long;
using namespace std;
const LL N = 2e5 + 5;
LL n, m, dfn[N], vis[N], low[N], cnt, num, st[N], top, bel[N], res[N], dis[N];
vector<pair<LL, LL>> g[N], v[N];
pair<LL, LL> e[N];
set<LL> path;
void tarjan(LL x, LL fa) {
    dfn[x] = low[x] = ++cnt;
    st[++top] = x;
    vis[x] = 1;
    for (auto [i, id] : g[x]) {
        if (id == fa) continue;
        if (!dfn[i]) {
            tarjan(i, id);
            low[x] = min(low[x], low[i]);
        } else if (vis[i]) {
            low[x] = min(low[x], dfn[i]);
        }
    }
    if (low[x] == dfn[x]) {
        num++;
        LL to;
        do {
            to = st[top--];
            bel[to] = num;
            vis[to] = 0;
        } while (to != x);
    }
}
priority_queue<tuple<LL, LL, LL>, vector<tuple<LL, LL, LL>>, greater<tuple<LL, LL, LL>>> q;
 
void dijkstra() {
    memset(dis, 0x3f, sizeof(dis));
    for (auto x : path) {
        auto [i, j] = e[x];
        if (res[i] == 0) {
            q.emplace(0, x, i);
            res[i] = x;
            dis[i] = 0;
        }
        if (res[j] == 0) {
            q.emplace(0, x, j);
            res[j] = x;
            dis[j] = 0;
        }
    }
    while (!q.empty()) {
        auto [cost, id, to] = q.top(); q.pop();
        if (dis[to] != cost) continue;
        for (auto [x, y] : g[to]) {
            if (dis[x] > dis[to] + 1 || (dis[x] == dis[to] + 1 && res[to] < res[x])) {
                dis[x] = dis[to] + 1;
                res[x] = res[to];
                q.emplace(dis[x], res[x], x);
            }
        }
    }
}
 
bool dfs(LL x, LL fa) {
    if (x == bel[n]) return true;
    for (auto [i, j] : v[x]) {
        if (i == fa) continue;
        if (dfs(i, x)) {
            path.insert(j);
            return true;
        }
    }
    return false;
}
 
void solve() {
    cin >> n >> m;
    cnt = num = top = 0;
    for (int i = 1; i <= n; i++) {
        dfn[i] = vis[i] = low[i] = bel[i] = res[i] = dis[i] = 0;
        g[i].clear();
        v[i].clear();
    }
    path.clear();
    for (int i = 1; i <= m; i++) {
        LL x, y;
        cin >> x >> y;
        e[i] = {x, y};
        g[x].emplace_back(y, i);
        g[y].emplace_back(x, i);
    }
    for (int i = 1; i <= n; i++) if (!dfn[i]) tarjan(i, -1);
    for (int i = 1; i <= m; i++) {
        LL x = e[i].first, y = e[i].second;
        if (bel[x] != bel[y]) {
            v[bel[x]].emplace_back(bel[y], i);
            v[bel[y]].emplace_back(bel[x], i);
        }
    }
    dfs(bel[1], 0);
    dijkstra();
    //cout << path.size() << endl;
    LL Q; cin >> Q;
    while (Q--) {
        LL x; cin >> x;
        cout << (res[x] == 0 ? -1 : res[x]) << ' ';
    }
    cout << endl;
}
int main(){
    ios::sync_with_stdio(false);
    cin.tie(0);
    int t;
    cin >> t;
    while (t--) {
        solve();
    }
    return 0;
}
```