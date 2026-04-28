## P11743
### Solution
模拟赛唯一一道切了的题。首先发现 $n\le 600$ 故考虑 $\mathcal{O}(n^3)$ 的做法。\
一开始想了不少思路，发现不管怎样都需要快速得到区间的总贡献，也就是说 $val_{i,j}$ 就是在区间 $[i,j]$ 里面放任意多个隔板后的最小代价，当然两边是必须放的，翻转后的就叫 $vall$ 也一样。这里求 $val_{i,j}$ 的方法就是先算出不放任何隔板的值，然后区间合并求最大值：
$$val_{i,j}=\max_{k=i}^{k<j} \space val_{i,k}+val_{k+1,j}$$

然后现在问题就转化为了要用一堆区间拼起来总长度为 $n$，最多选 $k$ 个翻转过的序列也就是 $k$ 次翻转操作，这个就是一个很简单的 $\mathcal{O}(n^3)$ 的动态规划了，设 $dp_{i,j}$ 表示在前 $i$ 个数中用了 $k$ 个翻转过的序列拼起来：
$$dp_{i,j}=\max(\max_{k=0}^{k<i}\space dp_{k,j}+val_{k+1,i},\space \max_{k=0}^{k<i}\space dp_{k,j-1}+vall_{k+1,i})$$
转移就是大概枚举上一个区间的结束位置然后拼上，有翻转或者不翻转两种选择。

### Code
```cpp
#include<bits/stdc++.h>
using namespace std;
using LL=long long;
LL n,a[605],val[605][605],vall[605][605],dp[605][605],m;
int main(){
    ios::sync_with_stdio(false);cin.tie(nullptr);cout.tie(nullptr);
    cin>>n>>m;
    for(int i=1;i<=n;i++){
        cin>>a[i];
    }
    for(int i=1;i<=n;i++){
        for(int j=i;j<=n;j++){
            for(int k=i;k<=j;k++) val[i][j]+=a[k]*(k-i+1),vall[i][j]+=a[k]*(j-k+1);
        }
    }
    for(int i=1;i<=n;i++){
        for(int j=i;j<=n;j++){
            for(int k=i;k<j;k++){
                vall[i][j]=max(vall[i][j],vall[i][k]+vall[k+1][j]);
            }
        }
    }
    for(LL i=1;i<=n;i++){
        for(int k=0;k<=m;k++) dp[i][k]=-1e14;
        for(int j=0;j<i;j++){
            for(int k=0;k<=min(i,m);k++){
                dp[i][k]=max(dp[i][k],dp[j][k]+val[j+1][i]);
            }
        }
        for(int j=0;j<i;j++){
            for(int k=1;k<=min(i,m);k++){
                dp[i][k]=max(dp[i][k],dp[j][k-1]+vall[j+1][i]);
            }
        }
    }
    LL ans=-1e14;
    for(int i=1;i<=m;i++) ans=max(ans,dp[n][i]);
    cout<<ans;
    return 0;
}
```