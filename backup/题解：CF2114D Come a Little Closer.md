## CF2114D

### Solution

直接按照题意移动，枚举每一个移动的点，可以理解为把这个点塞到了长方形中，也就是删掉，然后每次都计算面积。\
这里用 `multiset` 非常方便：

- 先把每个 $x_i$，$y_i$ 插入。
- 枚举删除的点，先删掉。
- 判断两个 `multiset` 是否为空，是则跳过。
- 计算长宽，也就是最大值与最小值之差，更新面积。
- 如果塞不进去，就安在边上。
- 最后再重新插入回去。

### Code

```cpp
#include<bits/stdc++.h>
using namespace std;
using LL = long long;
const LL N = 2e5 + 5;
LL X[N], Y[N], n, ans;
multiset<LL> cnt1, cnt2;

void solve(){
    cnt1.clear(), cnt2.clear();
    ans = 1e18 + 5; // 初始化一个非常大的值
    cin >> n;
    for (int i = 1; i <= n; i++) {
        cin >> X[i] >> Y[i];
        cnt1.insert(X[i]);
        cnt2.insert(Y[i]);
    }
    
    LL w = *cnt1.rbegin() - *cnt1.begin() + 1, h = *cnt2.rbegin() - *cnt2.begin() + 1;
    ans = min(ans, w * h);

    for (int i = 1; i <= n; i++) {
        cnt1.erase(cnt1.find(X[i]));
        cnt2.erase(cnt2.find(Y[i]));
        if (cnt1.empty() || cnt2.empty()) {
            cnt1.insert(X[i]);
            cnt2.insert(Y[i]);
            continue;
        }
        w = *cnt1.rbegin() - *cnt1.begin() + 1;
        h = *cnt2.rbegin() - *cnt2.begin() + 1;
        ans = min(ans, w * h);
        if (ans == n - 1) {
            ans += min(w, h);
        }
        cnt1.insert(X[i]);
        cnt2.insert(Y[i]);
    }

    cout << ans << endl;
}

int main(){
    LL t;
    cin >> t;
    while (t--) {
        solve();
    }
    return 0;
}

```