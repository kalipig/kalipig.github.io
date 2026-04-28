## UVa1635

### Solution
相邻两个求和，最终结果明显是 $\sum_{i=1}^{n} a_i \binom{n-1}{i-1}$（后面其实是杨辉三角，可以自己推一下）。
要 $a_i$ 不影响结果，那么最终的和一定能把 $m$ 整除，所以可以发现条件是当且仅当 $\binom{n-1}{i-1} \mod{m} =0$。

接下来就找这些 $i$。

首先会想到用乘法逆元求组合数，但是会发现根本不能取模，也没有保证 $m$ 是指数。
那么我们只能从质因数的方向来看看能不能整除。

我们先把 $m$ 分解质因数，技巧是存每个质数和出现次数 $e_t$，枚举每个因数然后除多次直到除不了，不用担心这个因数不是质因数，因为如果它有因数那也一定被除了，具体可以看代码。\
分完就是这个样子：$m= \prod_{j=1}^{k}p_{j}^{e^j}$。

要判断是否被 $p_{j}^{e}$ 整除，只需要比较他们的每一个 $p_j$ 的指数，如果 $m$ 里的指数大就可以。

我们知道组合数是一堆阶乘的乘除，所以预处理 $v_{i,j}$ 表示在 $i$ 的阶乘里有多少 $p_j$。
因为阶乘是上一个阶乘乘一个数，所以这个数组可以用前缀和的方法快速求出，算组合数就不用单独算了。

最后就来比较每个质因数的幂大小，由组合数公式可得：
$$v_{t,{ \binom{n-1}{i-1} }}=v_{t,n-1} - v_{t,i-1} - v_{t,n-1-j} = x$$
如果对于 $\forall t$，$x\ge e_t$ 都成立，那么这个 $i$ 就可以作为答案。

### Code
```cpp
#include <bits/stdc++.h>
using namespace std;
using LL = long long;
LL n,m;
vector<pair<LL,LL> > g;
vector<LL> ans;
LL v[1005][100005];  
int main(){
    ios::sync_with_stdio(false); cin.tie(nullptr);cout.tie(nullptr);
    cin>>n>>m;
    LL x=m;
    for(int p=2; (LL)p*p<=x; p++){
        if(x%p==0){
          int e=0;
          while(x%p==0){ x/=p; e++; }
          g.push_back({p,e});
        }
    }
    if(x>1) g.push_back({x,1});
    LL len=g.size();
    for(int t=0; t<len; t++){
      LL p=g[t].first;
      v[t][0]=0;
      for(int i=1; i<n; i++){
        LL c=0, x=i;
        while(x%p==0){ x/=p; c++; }
        v[t][i]=v[t][i-1]+c;
      }
    }
    for(int j=0; j<n; j++){
      bool flag=true;
      for(int t=0; t<len; t++){
        LL tot = v[t][n-1] - v[t][j] - v[t][n-1-j];
        if(tot<g[t].second){ flag=0; break; }
      }
      if(flag) ans.push_back(j+1);
    }
    cout<<ans.size()<<endl;
    for(LL i:ans) cout<<i<<" ";
    return 0;
}

```