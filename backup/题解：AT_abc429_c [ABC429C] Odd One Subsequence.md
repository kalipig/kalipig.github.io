## ABC429C
### Solution
首先可以想到用两个相等的数来确定三元组，另一个数就可以随便找一个不同的。\
现在大小为 $i$ 的数有 $cnt_i$ 个，那么对于每个 $i$，如果 $cnt_i\ge 2$ 就是可以当作两个相同的数的，所以选两个相同的数的方案就是 $\frac{cnt_i \times (cnt_i-1)}{2}$，然后剩下一个数就是随便选一个不同的就是 $n-cnt_i$，乘起来就是答案。

### Code
```cpp
#include<bits/stdc++.h>
using namespace std;
int main() {
    int N;
    cin >> N;
    vector<int> cnt(N + 1, 0);
    for (int i = 0; i < N; i++) {
        int x;
        cin >> x;
        cnt[x]++;
    }

    long long ans = 0;
    for (int a = 1; a <= N; a++) {
        if (cnt[a] >= 2) {
            long long cho = (long long)cnt[a] * (cnt[a] - 1) / 2;
            ans += cho * (N - cnt[a]);
        }
    }
    cout << ans;
}
```