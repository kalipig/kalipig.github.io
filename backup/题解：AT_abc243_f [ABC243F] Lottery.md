## abc243f

### Solution

考虑动态规划，$dp_{i,j,k}$ 表示在前 $i$ 个物品中，抽 $j$ 次，有 $k$ 个不同奖品的概率。

预处理一下：

- 组合数 $comb_{i,j}$ 表示在从 $i$ 个不同元素中取出 $j$ 个元素的所有组合的个数。
- 乘法逆元，这里采用快速幂法（关于为什么不用拓展欧几里得，因为别的地方要有快速幂）。

初始化：$dp_{0,0,0}=1$（显然）。

##### 动态规划过程

```cpp
for(int i=1;i<=n;i++){//枚举前i个。
    for(int j=0;j<=k;j++){//抽了j次。
        for(int p=0;p<=m;p++){//如果不抽第i个，有p个不同奖品的个数。
            dp[i][j][p]=dp[i-1][j][p];//不抽i，和上一个一样。
        }
     }
     for(int j=1;j<=k;j++){//抽了j次。
        for(int p=1;p<=m;p++){//有p个不同奖品的个数。
           for(int q=1;q<=j;q++){//在j次中，有q次抽到了i,至少一次。
               LL qt=qpow(a[i]*mmi(sum)%mod,q);//q次抽到i,就是抽到一次的概率的q次方。
               LL way=comb[j][q];//在这j次中，q次抽到i的组合。
               dp[i][j][p]=(dp[i][j][p]+dp[i-1][j-q][p-1]*way%mod*qt%mod)%mod;//不抽i的方案加上抽i的方案，注意取模。
            }
         }
     }
}
```

### Code

```cpp
#include<bits/stdc++.h>
using namespace std;
using LL=long long;
const LL mod=998244353;
LL n,m,k,dp[55][55][55]={1},comb[55][55],a[55],sum;// dp[i][j][k]：考虑前 i 个奖品，抽了 j 次，有 p 个不同奖品的概率
void combination(){
    for(int i=0;i<=50;i++){
        comb[i][0]=1;
        for(int j=1;j<=i;j++){
            comb[i][j]=(comb[i-1][j]+comb[i-1][j-1])%mod;
        }
    }
}
inline LL qpow(LL x,LL y){
    LL res=1;
    while(y){
        if(y&1) res*=x;
        x*=x;y/=2;
        x%=mod,res%=mod;
    }
    return res;
}
inline LL mmi(LL x){
    return qpow(x,mod-2);
}
void DP(){
    //enum i
    for(int i=1;i<=n;i++){
        for(int j=0;j<=k;j++){
            for(int p=0;p<=m;p++){
                dp[i][j][p]=dp[i-1][j][p];
            }
        }
        for(int j=1;j<=k;j++){
            for(int p=1;p<=m;p++){
                for(int q=1;q<=j;q++){
                    LL qt=qpow(a[i]*mmi(sum)%mod,q);
                    LL way=comb[j][q];
                    dp[i][j][p]=(dp[i][j][p]+dp[i-1][j-q][p-1]*way%mod*qt%mod)%mod;
                }
            }
        }
    }
}
int main(){
    cin>>n>>m>>k;
    for(int i=1;i<=n;i++){
        cin>>a[i];sum+=a[i];
    }
    combination();
    DP();
    cout<<dp[n][k][m];
    return  0;
}
```