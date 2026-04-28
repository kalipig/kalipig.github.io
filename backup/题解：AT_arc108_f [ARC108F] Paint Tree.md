# arc108f
## Problem
[problem](https://atcoder.jp/contests/arc108/tasks/arc108_f)

## Solution
看到两点的最长距离，很容易想到树的直径，这里先处理出起点，终点，以及到每个点的距离：$st,ed,dst[v],ded[v]$。\
现在很容易发现如果 $st,ed$ 是同一个颜色的时候答案就是直径。

现在考虑 $st$ 为黑，$ed$ 为白的情况，我们从最大值开始枚举答案 $mx$，如果 $\max(dst[v],ded[v])\le mx$，则 $v$ 的颜色不影响答案，否则与远的那个端点。\
很明显如果有一个点 $\min(dst[v],ded[v])>mx$，那么就不合法，所以在枚举的时候可以从这个从直径长度到最大的 $\min(dst[v],ded[v])$。

接下来看怎么计算不合法的答案。\
首先用一个 $f_i$ 数组记录一下按照 $dis[i]$ 从大到小的长度。然后在枚举答案时维护一个 $j$ 表示最大的一个小于答案的位置。所以有 $2^{n-j+2}$ 的答案不合法。

## Code
~~~cpp
#include<bits/stdc++.h>
using namespace std;
using LL=long long;
const LL N=1e6+5,mod=1e9+7;
namespace tree{
    vector<LL> g[N];
    LL st,ed,dst[N],ded[N];
    void get_st(LL x,LL fa){
        for(auto i : g[x]){
            if(i==fa) continue;
            dst[i]=dst[x]+1;
            if(dst[i]>dst[st]) st=i;
            get_st(i,x);
        }
    }
    void get_ed(LL x,LL fa){
        for(auto i : g[x]){
            if(i==fa) continue;
            ded[i]=ded[x]+1;
            if(ded[i]>ded[ed]) ed=i;
            get_ed(i,x);
        }
    }
}
using namespace tree;
LL n,dis[N],pw[N]={1},f[N],s,ans,mn=0;
bool cmp(LL p,LL q){
    return dis[p]>dis[q];
}
int main(){
	ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
    cin>>n;
    for(int i=1;i<=n;i++) pw[i]=pw[i-1]*2%mod;
    for(int i=1,u,v;i<n;i++){
        cin>>u>>v;
        g[u].emplace_back(v);
        g[v].emplace_back(u);
    }
    get_st(1,0);
    dst[st]=0;
    get_st(st,0);
    ed=st;
    get_ed(ed,0);
    s=ded[ed];
    //cout<<st<<' '<<ed<<endl;
    for(int i=1;i<=n;i++){
        f[i]=i,dis[i]=max(dst[i],ded[i]),mn=max(mn,min(dst[i],ded[i]));
    }
    sort(f+1,f+n+1,cmp);
    ans=s*pw[n]%mod,s--;
    //cout<<mn;
    LL j=1;
    while((s--)>=mn){
        while(j<=n&&s<dis[f[j]]) j++;
		ans=(ans-pw[n-j+2])%mod;
    }
    cout<<(ans+mod)%mod;
    return 0;
}
~~~