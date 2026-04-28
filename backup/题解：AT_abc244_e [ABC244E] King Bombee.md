## abc244e
### Problem
给定整数 $K$，$S$，$T$，$X$。满足以下条件的序列 $A=(A_0,A_1,\dots, A_K)$ 有多少个？
- $A_i$ 是一个介于 $1$ 和 $N$ 之间的整数（包括两端）。
- $A_0=S$。
- $A_K=T$。
- 存在一条边直接连接顶点 $A_i$ 和顶点 $A_{i+1}$。
- 整数 $X$ 在序列 $A$ 中出现偶数次（可能为零）。

### Solution
考虑图上动态规划，可以很快发现有 $dp_{k,v,cnt}$ 表示走了 $k$ 步到了 $v$ 已经出现了 $x$ 次。\
但是这样的 $n^3$ 复杂度无法通过。我们发现最后一维是没有实际意义的，只需要看是否为偶数。所以有：
~~~cpp
    LL nk=k+1,nv=to,ni=i;
    if(to==x) ni^=1;
    dp[nk][nv][ni]+=dp[k][j][i];
~~~
答案为 $dp_{K,T,0}$。

### Code
~~~cpp
#include<bits/stdc++.h>
using namespace std;
using LL=long long;
const LL mod = 998244353;
LL dp[2005][2005][2],n,m,K,s,t,x;
vector<LL> g[2005];
int main(){
    cin>>n>>m>>K>>s>>t>>x;
	dp[0][s][0]=1;
    for(int u,v,i=1;i<=m;i++){
        cin>>u>>v;
        g[u].push_back(v);
        g[v].push_back(u);
    }
    for(int k=0;k<K;k++)
		for(int j=1;j<=n;j++)
			for(int i=0;i<=1;i++) {
        	for (auto to : g[j]) {
            	LL nk=k+1,nv=to,ni=i;
            	if(to==x) ni^=1;
            	dp[nk][nv][ni]+=dp[k][j][i];//多一个，下一个位置，出现。
				dp[nk][nv][ni]%=mod;
        	}
    }
    cout<<dp[K][t][0];
    return 0;
}

~~~