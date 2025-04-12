## abc242f

### Problem

在 $n$ 行 $m$ 列的棋盘上放 $b$ 只黑车和 $w$ 只白车，要求每对黑车和白车都不在同一行或列上。

### Solution

考虑计数 dp，定义 $f[i][j]$ 表示把所有的车放在前 $i$ 行 $j$ 列里的方案数

以黑车为例，容易想到 $f[i][j]=C^{b}_{i\times j}$，但是这明显有一些方案不合法，因为实际上根本没有填满。

如果只填了 $p$ 行 $q$ 列，那么就有 $C_{i}^{x}\times C_{j}^{q}$ 中选的方法，且有 $f[p][q]$ 种填的方法。

所以不合法的有：
$$
C_{i}^{x}\times C_{j}^{q}\times f[p][q]
$$
然后把两个 $f$ 数组都处理处来之后我们考虑这么求答案。

还是一样的，容易想到答案为 $f[n][m]$，但是实际上我们要考虑两种车之间的限制，即不能同时填一行或一列。

所以如果黑车填了 $i$ 行 $j$ 列，那么白车则会填剩下 $n-i$ 行 $m-j$ 列中的 $p$ 行 $q$ 列。

所以把两个车的方案乘起来就好了：

~~~cpp
	LL tmp1=c[n][i]*c[n-i][p]%mod;
	LL tmp2=c[m][j]*c[m-j][q]%mod;
	ans=(ans+tmp1*tmp2%mod*f1[i][j]%mod*f2[p][q]+mod)%mod;
~~~

### Code

**注意取模！！！**

~~~cpp
#include<bits/stdc++.h>
using namespace std;
using LL=long long;
const LL mod=998244353;
LL c[5005][5005]={1},f1[5005][5005],f2[5005][5005],n,m,b,w,ans;
int main(){
    cin>>n>>m>>b>>w;
    for(int i=1;i<=3000;i++){
        c[i][0]=1;
        for(int j=1;j<=3000;j++){
            c[i][j]=(c[i-1][j-1]+c[i-1][j])%mod;
        }
    }
    for(int i=1;i<=n;i++){
        for(int j=1;j<=m;j++){
            f1[i][j]=c[i*j][b];
            f2[i][j]=c[i*j][w];
        }
    }
    for(int i=1;i<=n;i++){
        for(int j=1;j<=m;j++){
            for(int p=1;p<=i;p++){
                for(int q=1;q<=j;q++){
                    if(i==p&&j==q) continue;
                    f1[i][j]=(f1[i][j]-f1[p][q]*c[i][p]%mod*c[j][q]%mod)%mod;
                }
            }
        }
    }
    for(int i=1;i<=n;i++){
        for(int j=1;j<=m;j++){
            for(int p=1;p<=i;p++){
                for(int q=1;q<=j;q++){
                    if(i==p&&j==q) continue;
                    f2[i][j]=(f2[i][j]-f2[p][q]*c[i][p]%mod*c[j][q]%mod)%mod;
                    //cout<<f2[i][j]<<' '<<endl;
                }
            }
        }
    }
    for(int i=1;i<=n;i++){
        for(int j=1;j<=m;j++){
            for(int p=1;p+i<=n;p++){
                for(int q=1;q+j<=m;q++){
                    LL tmp1=c[n][i]*c[n-i][p]%mod;
                    LL tmp2=c[m][j]*c[m-j][q]%mod;
                    ans=(ans+tmp1*tmp2%mod*f1[i][j]%mod*f2[p][q]+mod)%mod;
                }
            }
        }
    }
    cout<<(ans%mod+mod)%mod;
    return 0;
}

~~~