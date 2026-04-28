## CF2152C
### Solution
对于一个三元组，只有两种情况：
- 先是两个连续的，后面还有一个。这个时候的代价就是 $1$。
- $0$ 和 $1$ 交替出现，不管你要删哪个代价都是 $2$。删了就会变成第一种情况。

所以对于一个查询的区间，先判读里面的 $0$ 和 $1$ 的个数是不是 $3$ 的倍数，然后判断它是不是属于第二种情况。如果是的话答案加一，然后剩下就是三元组的个数加上就好了。
### Code
```cpp
#include<bits/stdc++.h>
using namespace std;
using LL=long long;
LL n,a[500005]={-1},cnt0[500005],cnt1[500005],q,b[500005];
void solve(){
    memset(b,0,sizeof(b));
    cin >> n>>q;
    for(int i=1;i<=n;i++){
        cin>>a[i];
        if(a[i]!=a[i-1])b[i]=1;
        if(a[i]) cnt1[i]=cnt1[i-1]+1,cnt0[i]=cnt0[i-1];
        else cnt0[i]=cnt0[i-1]+1,cnt1[i]=cnt1[i-1];
    }
    for(int i=1;i<=n;i++){
        b[i]+=b[i-1];
    }
    while(q--){
        LL l,r,res=0;
        cin>>l>>r;
        
        if((r-l+1<3)||((cnt0[r]-cnt0[l-1])%3)||((cnt1[r]-cnt1[l-1]))%3){
            cout << "-1\n";
            continue;
        }
        if(b[r]-b[l-1]==r-l+(a[l]!=a[l-1])){
            res=1;
        }
        cout<<res+(cnt0[r]-cnt0[l-1])/3+(cnt1[r]-cnt1[l-1])/3<<endl;
    }

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