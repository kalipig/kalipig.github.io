## ABC242E
### Problem
给你一个字符串，要求有多少个一样长的回文串小于它。

### Solution
因为回文字符串的两边是一样的，所以我们只需要先看字符串的一半，然后判断一下是左边一半大还是右边。
~~~cpp
    string tmp=s.substr(0,n/2);
    reverse(tmp.begin(),tmp.end());
    if(s>=(s.substr(0,(n+1)/2)+tmp)) ans++;
~~~
对于前面的正常统计 $1\Rightarrow \cfrac{(n+1)}{2}$ 就好了：
$$ans=ans\times26+s_i$$

### Code
应该挺好看的吧。
~~~cpp
#include<bits/stdc++.h>
using namespace std;
using LL=long long;
const LL mod=998244353;
LL n,q;
string s;
int main(){
    cin>>q;
    while(q--){
        LL ans=0;
        cin>>n>>s;
        for(int i=0;i<(n+1)/2;i++){
            ans*=26;
            ans=(ans+LL(s[i]-'A'))%mod;
        }
        string tmp=s.substr(0,(n)/2);
        reverse(tmp.begin(),tmp.end());
        if(s>=(s.substr(0,(n+1)/2)+tmp)) ans++;
        cout<<ans%mod<<endl;
    }
    return 0;
}
~~~