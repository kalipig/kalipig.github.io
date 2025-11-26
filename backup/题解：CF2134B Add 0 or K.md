## CF2134B

### Problem

给你一个数组 $n$ 和一个数 $k$，让你给每个 $a_i$ 加上 $p\times k$ 这里 $0\le p\le k$，使得所有 $a_i$ 的 $\gcd$ 最小，输出一个方案。

### Solution

设这个 $\gcd$ 为 $x$。首先可以证明 $\gcd(k,x)=1$，因为如果大于一，那么所有的原来的 $a_i$ 一定包含了这个 $\gcd$。

我们找到最小的 $x$，关于为什么最小的就行后面说，这里直接枚举就好了，只是找到互质的数应该不是很大，这个就不证明了。

然后我们就要计算对于 $a_i$ 要加上多少个 $k$，是 $(((x-(a_i\bmod x))\bmod x)\times inv)\bmod x$  其中 $inv$ 是乘法逆元，这个其实就是补上 $a_i$ 使它能被 $x$ 整除。

那么怎么保证 $(((x-(a_i\bmod x))\bmod x)\times inv)\bmod x$ 一定比 $k$ 小呢，因为对 $x$ 取模了。

$k=1$ 的情况记得单独处理。

### Code

```cpp
#include <iostream>
#include <cstdio>
#include <cstring>
#include <algorithm>
#include <vector>
#include <map>
#include <queue>
#include <iomanip>
using namespace std;
using LL=long long;
LL n,k,a[100005];
LL invv(LL a, LL m)
{
    for(LL x=1;x<m;x++)
    {
        if((a*x)%m==1)
        {
            return x;
        }
    }
    return 1;
}
inline LL gcd(LL x,LL y){
    if(!y) return x;
    return gcd(y,x%y);
}
void solve(){
    LL g=2;
    cin>>n>>k;
    for(int i=0;i<n;i++)
    {
        cin>>a[i];
    }
    if(k==1)
    {
        for(int i=0;i<n;i++)
        {
            if(a[i]%2==1)
            {
                cout<<a[i]+1<<" ";
            }
            else
            {
                cout<<a[i]<<" ";
            }
        }
        cout<<endl;
        return ;
    }
    while(gcd(k,g)!=1)
    {
        g++;
    }
    LL ink=invv(k%g,g);
    for(int i=0;i<n;i++)
    {
        cout<<a[i]+((((g-a[i]%g)%g)*ink)%g)*k<<" ";
    }
    cout<<endl;
}
int main(){
    int t;
    cin>>t;
    while(t--) solve();
    return 0;
}
```