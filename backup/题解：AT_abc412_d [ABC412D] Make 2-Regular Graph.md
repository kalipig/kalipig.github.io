## abc412d
### Solution
这个数据范围一眼爆搜。\
对于现在搜索到的这个点 $x$，会有三种情况，需要连，刚好不用操作和需要删。\
所以我们可以直接不分类讨论，因为 $n\le 8$，不管怎么样都把连或者删试一下，回溯就好了。
### Code
```cpp
#include <bits/stdc++.h>
using namespace std;
using LL=long long;
LL n, m, ans = 1e18;
bool b[15][15], vis[15], g[15][15];
LL calc() {
	LL cnt = 0;
	for (int i = 1; i <= n; i++){
		for (int j = i + 1; j <= n; j++){
			if (b[i][j] && !g[i][j] || !b[i][j] && g[i][j])cnt++;
		}
	}
	return cnt;
}
inline void dfs(LL x) {
	if (x == n + 1) {
		ans = min(ans, calc());
		return;
	}
	for (int i = 1; i <= n; i++)
		if (!vis[i] && x != i && !g[x][i] && !g[i][x]) {
			vis[i] = g[x][i] = g[i][x] = 1;
			dfs(x + 1);
			vis[i] = g[x][i] = g[i][x] = 0;
		}
}
int main() {
	cin>>n>>m;
	for (int i = 1,u,v; i <= m; i++) {
		cin>>u>>v;
		b[u][v] = b[v][u] = 1;
	}
	dfs(1);
	cout<<ans;
	return 0;
}

```