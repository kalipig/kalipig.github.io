## CF2132C1

### Solution
想要购买的次数最小，很明显每次购买的要最多。\
那么我们可以先预处理出 $3$ 的幂，然后从大到小，能买大一点的 $x$ 就尽量买，把答案加起来就好了。

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
LL g[25]={1};
void solve(){
    LL x,ans=0;
    cin>>x;
    for(int i=20;i>=0;i--){
        while(x>=g[i]){
            x-=g[i];
            if(i)ans+=g[i+1]+i*g[i-1];
            else ans+=g[i+1];
        }
    }
    cout<<ans<<endl;
}
int main() {
    for(int i=1;i<=20;i++) g[i]=3ll*g[i-1];
    int t;
    cin>>t;
    while(t--) solve();
    return 0;
}
```