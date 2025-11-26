## CF2152B
### Solution
首先很明显 $K$ 要背朝 $D$ 的方向逃跑，那么这样就一定会先到达一个边界。\
这个时候 $K$ 就可以横着走了，但是要注意 $D$ 是可以斜着走的。\
所以横着走是没有用的，因为先竖着再横着其实步数和斜着的 $D$ 其实是一样的。\
我们只需要找出那个最远的离边界的距离就好了。
### Code
```cpp
#include<bits/stdc++.h>
using namespace std;
using LL=long long;
LL n,a,b,c,d;
void solve(){
    cin>>n >> a >> b >> c >> d;
    bool l=(a<c),u=(b<d);
    LL dis=0;
    if(l&&a!=c) dis=max(dis,c);
    if(!l&&a!=c) dis=max(dis,n-c);
    if(u&&b!=d) dis=max(dis,d);
    if(!u&&b!=d) dis=max(dis,n-d);
    cout << dis << endl;
}
int main(){
    int t;
    cin >> t;
    while(t--){
        solve();
    }
    return 0;
}
```