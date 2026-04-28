## CF2114E

### Solution

先看，这是一棵树，所以想树上动态规划。\
发现这个路径是从一号节点开始分奇和偶，所以可以定义 $dp_{0/1,i}$ 表示第 $i$ 个节点在偶数或者奇数层的最大值。
对于一个点 $x$ 和它的父亲 $fa$，所以可以根据题目中的定义有：
$$\begin{cases}dp_{0,x}=\max(-val_x,-val_x+dp_{1,fa})\\dp_{1,x}=\max(val_x,val_x+dp_{1,fa})\end{cases}$$
### Code

```cpp
#include<bits/stdc++.h>
using namespace std;
using LL=long long;
const LL N=2e5+5;
LL t,n, dp[2][N],val[N];
vector<LL> g[N];
void dfs(LL x,LL fa){
    dp[1][x]=max(val[x],val[x]+dp[0][fa]),dp[0][x]=max(-val[x],-val[x]+dp[1][fa]);
    for(auto i : g[x]){
        if(i==fa) continue;
        dfs(i,x);
    }
}
void solve(){
    memset(dp,0,sizeof(dp));
    cin>>n;
    for(int i=1;i<=n;i++){
        cin>>val[i];
        g[i].clear();
    }
    for(int i=1,x,y;i<n;i++){
        cin>>x>>y;
        g[x].emplace_back(y);
        g[y].emplace_back(x);
    }
    dp[0][1]=val[1];
    dp[1][1]=-val[1];
    dfs(1,0);
    for(int i=1;i<=n;i++) cout<<dp[1][i]<<' ';
    cout<<endl;
}
int main(){
    cin>>t;
    while(t--){
        solve();
    }
}
```