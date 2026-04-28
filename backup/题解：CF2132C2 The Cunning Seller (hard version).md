## CF2132C2

### Solution
观察题目中的花费的式子 $3^{x+1}+x\times 3^{x-1}$。\
发现它与 $x$ 是指数级增长，所以很明显每次小的购买数量肯定是性价比更优的，可以证明：设 $y=x-1$ 也就是只卖原来三分之一的数量。\
那么花费就是 $3\times(3^y+y\times 3^{y-1})=3\times(3^{x-1}+x\times 3^{x-2}-3^{x-2})=3^x+x\times 3^{x-1}-3^{x-1}$ 这个明显是小于 $3^{x+1}+x\times 3^{x-1}$ 的。\
所以我们要尽量每次买的少一点，但是还有购买次数的限制。
在上一道题 [C1](https://www.luogu.com.cn/problem/CF2132C1) 里面求出了最小的购买次数，那么现在对于 $k$ 次购买的限制，如果连最小的次数都不能满足，就是肯定无解的。\
否则可以把买一次 $3^x$ 个西瓜转化为买 $3$ 次 $3^{x-1}$ 个西瓜，这样会多两次购买，但是更划算，把剩下的 $k$ 次用完就好了。

### Code
```cpp
#include <bits/stdc++.h>
using namespace std;
using LL= long long;
LL n,k,cnt[30],p[30]={1};
vector<pair<LL, LL>> a;
void solve() {
    a.clear();
	cin >> n >> k;
	for (int i=0;i<=20;i++) {
		a.emplace_back( p[i],p[i+1]+i*(i?p[i-1]:1));
	}
	LL m = a.size();
	for (int i = m - 1; i >= 0;i--) {
		cnt[i] = n / a[i].first;
		n %= a[i].first;
		k -= cnt[i];
	}
	if (k<0) {
		 cout << -1 << "\n";
		return;
	}
	for(int i = m - 1; i > 0;i--) {
		LL t =  min(k / 2, cnt[i]);
		cnt[i] -= t;
		cnt[i - 1] += t * 3;
		k -= t * 2;
	}
	LL ans = 0;
	for (int i = 0; i < m; i++) {
		ans += a[i].second * cnt[i];
	}
	cout << ans <<endl;
}

int main() {
    for(int i=1;i<=20;i++) p[i]=p[i-1]*3;
	int t;
	cin >> t;
	while (t -- ) solve();
	return 0;
}

```