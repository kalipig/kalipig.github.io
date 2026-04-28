## CF2134C

### Solution

这个题看到子数组感觉很多，然后跟你 $n\le2\times 10^5$ 让人感觉是个 $n \log n$ 的做法，所以赛时想到贪心但是感觉很假，但是确实是贪心。

首先很明显的是，对于一个子数组 $l-r$ 这个范围，你如果确保了 $l-k,l\le k <r$ 是个满足条件的数组，那么你只需要考虑第 $k$ 位并且 $k$ 还是奇数。

通俗的说，就是前面的偶数位置的和要大于奇数位置了，那么后面的部分就不用考虑前面了，因为就算你加上了偶数位置一定更大。

所以我们只需要先计算两个奇数位置夹着一个偶数位置，然后看一下分别每一个偶数位置和前面的，后面的差就好了。

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
LL n,f[500005],a[500005];
void solve(){
    cin>>n;
    for(int i=1;i<=n;i++) cin>>a[i];
    LL ans=0;
    for(int i=2;i<n;i+=2){
        if(a[i]<a[i-1]+a[i+1]){
            LL k=a[i-1]+a[i+1]-a[i];
            if(a[i+1]>=k){
                a[i+1]-=k;
                ans+=k;
            } else{
                ans+=a[i+1];
                LL temp=k-a[i+1];
                a[i+1]=0;a[i-1]-=temp;
                ans+=temp;
            }
        }
    }
    for(int i=1;i<n;i+=2){
        if(a[i]>a[i+1]){
            ans+=a[i]-a[i+1];
            a[i]=a[i+1];
        }
    }
    for(int i=2;i<n;i+=2){
        if(a[i]<a[i+1]){
            ans+=a[i+1]-a[i];
            a[i+1]=a[i];
        }
    }
    cout<<ans<<endl;
}

int main(){
    int t;
    cin>>t;
    while(t--) solve();
    return 0;
}
```