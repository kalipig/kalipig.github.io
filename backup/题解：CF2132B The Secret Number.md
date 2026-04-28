## CF2132B
### Solution
在 $x$ 后面加上 $k$ 个 $0$，就是 $x*10^k$。\
所以题目就转成了给你 $x\times 10^k+x$，求所有可能的 $x$。

我们发现这个数是小于 $10^{18}$ 的。所以我们枚举 $k$ 从 $1$ 到 $18$。然后因为 $x\times 10^k+x=x\times (10^k+1)$，所以就看看 $x$ 能否整除 $10^k+1$，如果可以就除然后加入答案。最后记得排序。

### Code
```cpp
#include <iostream>
#include <cstdio>
#include <cmath>
#include <cstring>
#include <vector>
#include <algorithm>
#include <queue>
using namespace std;
using LL=long long;
LL g[20]={1};
void solve(){
    LL x;
    cin>>x;
    vector<LL> ans;
    for(int i=1;i<=18;i++){
        LL k=g[i]+1;
        if(x%k==0) ans.emplace_back(x/k);
    }
    cout<<ans.size()<<endl;
    sort(ans.begin(),ans.end());
    for(auto i : ans){
        cout<<i<<' ';
    }
    cout<<endl;
}
int main() {
    for(int i=1;i<=18;i++) g[i]=10ll*g[i-1];
    int t;
    cin>>t;
    while(t--) solve();
    return 0;
}
```