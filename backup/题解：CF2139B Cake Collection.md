## CF2139B

### Problem

给你 $n$ 个烤箱，每个烤箱每秒会增加 $a_i$ 个蛋糕，你要在前 $m$ 秒里每秒拿走一个烤箱的蛋糕，求最多能拿走多少。

### Solution

对于每个烤箱，在第 $k$ 秒可以有 $k\times a_i$ 个，会浪费 $(m-k)\times a_i$，明显每个烤箱去一次就够了。

要想拿到的最多，很明显可以贪心考虑。时间越靠后就去拿更大的，这样浪费的更少。

### Code

```cpp
#include<bits/stdc++.h>
using namespace std;
using LL=long long;
LL n,a[200005],m;
void solve(){
    LL ans=0;
    cin>>n>>m;
    for(int i=1;i<=n;i++) cin>>a[i];
    sort(a+1,a+n+1);
    for(int i=n;i>=1&&m;i--){
        ans+=a[i]*m;
        //cout<<ans<<' ';
        m--;
    }
    cout<<ans<<"\n";
}
int main(){
    int t;
    cin>>t;
    while(t--) solve();
    return 0;
}