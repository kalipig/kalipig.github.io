## CF2094E

### Solution

看到异或，可以很明显想到二进制。先枚举答案 $x$，对于 $x$ 的每一个二进制位，只有与它异或的另一个数的那一位与其相反才有用。

所以我们可以把每一位的 $1$ 的数量加起来，然后枚举答案时可以直接计算。

### Code

```cpp
#include <bits/stdc++.h>
using namespace std;
using LL = long long;
LL t;
LL n, ans, a[200005], f[31], g[35] = {1};
int main() {
	for (int i = 1; i <= 31; i++) g[i] = g[i - 1] * 2;
	cin >> t;
	while (t--) {
		memset(f, 0, sizeof(f));
		cin >> n;
		ans = 0;
		for (int i = 1; i <= n; i++) {
			cin >> a[i];
			LL p = a[i], cnt = 0;
			for (LL j = 30; j >= 0; j--) {
				if (p >= g[j]) {
					p -= g[j];
					f[j]++;
				}
			}
		}
		for (int i = 1; i <= n; i++) {
			LL sum = 0;
			for (int j = 0; j <= 30; j++) {
				sum += g[j] * ((a[i] & (1 << j)) ? (n - f[j]) : f[j]);
			}
			// cout<<a[i]<<' '<<sum<<endl;
			ans = max(ans, sum);
		}
		cout << ans << endl;
	}
	return 0;
}
```