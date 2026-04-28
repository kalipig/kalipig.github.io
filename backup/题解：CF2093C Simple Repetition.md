## CF2093C

### Solution

因为是把 $x$ 复制 $k$ 次，所以新的数一定有一个因数是 $x$，那么当 $k=1$ 时，只需要判断 $x$ 是不是质数就可以了。

然后就想想什么时候 $k$ 是质数，因为 $k$ 一定是一个由若干个零和一个一组成的循环节组成的数，枚举一下发现，只有 $11$ 是质数，其他都不是。

所以可以推出以下结论：

- 当 $k=1$，答案为 $x$ 是否为质数。
- 当 $k=2$ 且 $x=1$，答案为正确。
- 否则为错误。

### Code

~~~cpp
#include<bits/stdc++.h>
using namespace std;
using LL=long long;
LL n,k,t,x;
bool check(LL q){
    for(int i=2;i*i<=q;i++){
        if(!(q%i)) return 0;
    }
    return 1;
}
int main(){
    cin>>t;
    while(t--){
        cin>>x>>k;
        if(x==1){
            if(k==2) cout<<"YES\n";
            else cout<<"NO\n";
            continue;
        }
        if(x>1&&k>1) cout<<"NO\n";
        else if(k==1){
            if(check(x))cout<<"YES\n";
            else cout<<"NO\n";
        }
    }
    return 0;
}
~~~