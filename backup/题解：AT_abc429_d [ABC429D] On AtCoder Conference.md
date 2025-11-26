## ABC429D
### Solution
首先观察到池塘的周长很大，但是人数有限，所以首先对位置进行离散化并计数。\
很明显朴素的做法 $\mathcal{O}(n^2)$ 无法通过，但是因为只需要求出对于每个 $i$ 的答案的和，故考虑从 $i$ 到 $i+1$ 的答案的变化。

很明显，$i$ 答案所包含的区间与 $i+1$ 答案所包含的区间一定重和，因为下一次不包含第 $i$ 个位置的人，但是总人数依然要比 $c$ 大，所以区间右端点一定往后移动，转化为双指针问题。每次移动累加答案即可。

### Code
```cpp
#include <bits/stdc++.h>
using namespace std;
using LL = long long;

struct P { LL id, num; } p[500005];
LL n, m, c, a[500005], cnt, r, now, ans;
map<LL, LL> pl;

bool cmp(P x, P y) { return x.id < y.id; }

LL dis(LL l) {
    if (l == cnt) return p[1].id + m - p[l].id;
    return p[l + 1].id - p[l].id;
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    cin >> n >> m >> c;
    for (LL i = 1; i <= n; i++) {
        cin >> a[i];
        if (pl.count(a[i])) p[pl[a[i]]].num++;
        else {
            cnt++;
            p[cnt].id = a[i];
            p[cnt].num = 1;
            pl[a[i]] = cnt;
        }
    }
    sort(p + 1, p + cnt + 1, cmp);
    r = 1; now = p[1].num;
    for (LL l = 1; l <= cnt; l++) {
        now -= p[l].num;
        while (now < c) {
            r++;
            if (r > cnt) r -= cnt;
            now += p[r].num;
        }
        ans += now * dis(l);
    }
    cout << ans;
}

```