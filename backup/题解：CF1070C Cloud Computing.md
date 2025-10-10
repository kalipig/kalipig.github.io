## CF1070C

### Solution

这道题直接用线段树去加这几个操作很麻烦（主要是难写），所以我们考虑用权值线段树。\
用 $num,cost$ 分别记录数量和总花费。\
在输入时存下每个 $l,r$ 之间的花费，在 $l\rarr n$ 之间加上 $c$ 的花费，在 $r+1\rarr n$ 加上 $-c$ 的花费。

用 $ask$ 函数查询前 $k$ 便宜的方案，然后每天计算一下就好了，单点修改，区间查询。
具体计算：

- `LL id = tree.ask(k, 1, 1, P);`

  - &#x20;解释：\
    $id = ask(k)$ 其中，$ask(k)$ 表示在区间 $[1, P]$ 中找出第 $k$ 小的元素的索引，$P$ 是总长度。

- `ans += tree.query_sum(1, id - 1, 1, 1, P) + (k - tree.query_num(1, id - 1, 1, 1, P)) * id;`

  - &#x20;解释：
    $$
    ans += {sum}(1, {id} - 1) + \left( k - {num}(1, {id} - 1) \right) \times {id}
    $$
    其中：
    - ${sum}(1, {id} - 1)$ 表示区间 $[1, {id} - 1]$ 内所有花费的总和。
    - ${num}(1, {id} - 1)$ 表示区间 $[1, {id} - 1]$ 内所有个数的个数。
    - ${id}$ 是通过 ${ask}(k)$ 得到的索引。

别忘了，如果不够 $k$ 个，就直接算总和 $sum_1$。
### Code

```cpp
#include <bits/stdc++.h>
using LL = long long;
using namespace std;
const LL N = (LL)(1e6) + 5;
struct seg_tree {
    #define get_mid LL mid=(l+r)/2

    struct node {
        LL num, cost;
    }tr[4000005];

    inline void pushup(LL x) { 
        tr[x].num = tr[x << 1].num + tr[x << 1 | 1].num;
        tr[x].cost = tr[x << 1].cost + tr[x << 1 | 1].cost;
    }

    void update(LL d, LL L, LL x, LL l, LL r) {
        if (l == r) {
            tr[x].num += d;
            tr[x].cost = tr[x].num * l;
            return;
        }
        get_mid;
        if (L <= mid) update(d, L, x << 1, l, mid);
        else update(d, L, x << 1 | 1, mid + 1, r);
        pushup(x);
    }

    LL query_num(LL L, LL R, LL x, LL l, LL r) {
        if (L > R) return 0;
        if (L <= l && r <= R) return tr[x].num;
        get_mid;
        LL res = 0;
        if (L <= mid) res += query_num(L, R, x << 1, l, mid);
        if (R > mid) res += query_num(L, R, x << 1 | 1, mid + 1, r);
        return res;
    }

    LL query_sum(LL L, LL R, LL x, LL l, LL r) {
        if (L > R) return 0;
        if (L <= l && r <= R) return tr[x].cost;
        get_mid;
        LL res = 0;
        if (L <= mid) res += query_sum(L, R, x << 1, l, mid);
        if (R > mid) res += query_sum(L, R, x << 1 | 1, mid + 1, r);
        return res;
    }

    LL ask(LL k, LL x, LL l, LL r) {
        if (l == r) return r;
        get_mid;
        if (k <= tr[x << 1].num) return ask(k, x << 1, l, mid);
        else return ask(k - tr[x << 1].num, x << 1 | 1, mid + 1, r);
    }
}tree;

LL n, k, q, l, r, c, p, P;
vector<pair<LL, LL>> op[N];

int main() {
    cin >> n >> k >> q;
    for (int i = 1; i <= q; i++) {
        cin >> l >> r >> c >> p;
        op[l].push_back({p, c});
        op[r + 1].push_back({p, -c});
        P = max(P, p);
    }
    LL ans = 0;
    for (int i = 1; i <= n; i++) {
        for (auto j : op[i]) tree.update(j.second, j.first, 1, 1, P);
        if (tree.tr[1].num <= k) ans += tree.tr[1].cost;
        else {
            LL id = tree.ask(k, 1, 1, P);
            ans += tree.query_sum(1, id - 1, 1, 1, P) + (k - tree.query_num(1, id - 1, 1, 1, P)) * id;
        }
    }
    cout << ans << endl;
    return 0;
}

```