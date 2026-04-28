## CF2165C
### Solution
首先是第一个观察，就是既然 $b_i$ 可以为 $0$，而 $0$ 对异或的结果是没有影响的，所以**不一定要用完 $n$ 个数**。

然后要最后结果为 $k$，显然 $k$ 的每个二进制位只要有一个 $b_i$ 的这一位为 $1$ 就行。

如果 $k$ 的第 $i$ 二进制位也就是 $2^i$ 无法被任何一个 $b_i$ 取到就无法成立。所以现在要花费来使 $b_i$ 的上限变大。

肯定是贪心地选择最大的一个 $a_i$ 来使 $a_i$ 变大，然后可以取到的 $b_i$ 也就会变大，便可以覆盖到整个 $k$。

所以先把 $a$ 数组从大到小排序，枚举每一个 $2^i$，如果当前 $a$ 的值可以覆盖 $2^i$ 就直接用，否则补上并统计花费。

但是会发现这样有一个漏洞，就是比如 $a_i>2^i$，它剩下的 $a_i-2^i$ 依然可以作贡献，取最大值的时候也需要考虑，所以可以用一个优先队列来存这个用过的值，最多 $30$ 个也就是没位产生一个。

### Code
```cpp
#include<bits/stdc++.h>
using namespace std;
using LL=long long;
LL a[500005];

void solve(){
    LL n,q;
    cin>>n>>q;
    for(int i=1;i<=n;i++) cin>>a[i];
    sort(a+1,a+n+1,greater<LL>());
    
    while(q--){
        LL k;
        cin>>k;
        LL ans=0;
        priority_queue<LL,vector<LL> > q;
        int j=1;
        
        for(int i=(1<<30);i>=1;i>>=1){
            if(k>=i){
                LL mx;
                bool from_array=false;
                if(j<=n&&(q.empty()||a[j]>=q.top())){
                    mx=a[j];
                    from_array=true;
                }else{
                    mx=q.top();
                    from_array=false;
                }
                
                if(mx>i){
                    q.push(mx-i);
                    k-=i;
                }else{
                    ans+=i-mx;
                    //cout<<i<<' '<<mx;
                    k-=i;
                }
                if(from_array&&j<=n){
                    j++;
                }else if(!from_array&&!q.empty()){
                    q.pop(); 
                }
            }
        }
        cout<<ans<<"\n";
    }
}

int main(){
    ios_base::sync_with_stdio(false);
    cin.tie(nullptr);
    int t;
    cin>>t;
    while(t--) solve();
    return 0;
}
```