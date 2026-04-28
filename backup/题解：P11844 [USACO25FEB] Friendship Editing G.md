## P11184
## Solution
看到 $n\le 16$ 一眼状压，**我的方法一点都不抽象！！！**。\
先观察题目，为了方便状压，存图的时候选择对于一个点，用二进制的点集来表示其直接连边的点，即 $g_x=1<<y$。

要使整个图满足，我们发现可以把它划分为子点集然后合并，也就是从小到大转移状态。
然后想到 $dp_s$ 表示点集为 $s$ 时的最小答案，那么接下来考虑转移。

我们发现对于一个状态 $s$，要让它合法，它一定是对于另一个点 $z\notin s$，它当中的所有点到这个 $z$ 的最短路都为 $1$ 或 $2$。\
解释一下，因为题目要求确保以下性质成立所需的最小操作次数：如果奶牛 $a$ 和 $b$ 是朋友，则对于每头其他奶牛 $c$，$a$ 和 $b$ 中至少之一与 $c$ 是朋友。\
那么对于 $x\in s,y\in s$，先看最短路为 $1$ 的时候，你会发现很明显 $x$ 与 $z$ 是朋友，那么 $y$ 也必须与 $z$ 是朋友，所以最短路为 $1$。然后 $2$ 的情况就很好理解了，$x$ 和 $y$ 与 $z$ 没有关系。

发现这个性之后就可以动态规划了，我们用 $num_s$ 表示点集 $s$ 中的边数，$dp_s$ 表示要是其合法需要的最小操作次数。\
接下来看看动态规划的过程：
- 枚举从小到大的点集 $s$。
- 枚举 $s$ 的所有子集 $t$。
- $k$ 为 $t$ 的补集。
- 删的边数量是 $t$ 中和 $k$ 中点的数量的积。
- 连的边就是 $num_s-num_t-num_k$。
- 最后不上一个 $num_k$ 因为要把 $t$ 补成 $s$。
- 更新最小值。

### Code
```cpp
#include <bits/stdc++.h>
#define cnt1 __builtin_popcount
using namespace std;
using LL = long long;

LL n, m, mx, dp[1 << 19], U[20], V[20], g[20], num[1 << 19];

int main() {
    cin >> n >> m;
    for (int i = 1, x, y; i <= m; i++) {
        cin >> x >> y;
        g[x - 1] |= 1 << (y - 1);
        g[y - 1] |= 1 << (x - 1);
    }

    mx = (1 << n) - 1;

  
    for (int s = 0; s <= mx; s++) {
        for (int i = 0; i < n; i++) {
            if (s & (1 << i)) {
                num[s] += cnt1(g[i] & s);
            }
        }
        num[s] /= 2; 
        dp[s] = num[s]; 
    }

    for (int s = 0; s <= mx; s++) {
        for (int t = s; t; t = (t - 1) & s) {
            LL k = s ^ t;
            LL cost = dp[t] + cnt1(t) * cnt1(k) - (num[s] - num[t] - num[k]) + num[k];
            dp[s] = min(dp[s], cost);
        }
    }

    cout << dp[mx] << "\n";
    return 0;
}
```