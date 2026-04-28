## CF2093B

### Solution

可以发现，只要想删，一定可以做到把除了前导零和唯一一个非零数的数全部删了，此时的值就是唯一一个数加上前导零除以它自己也就是 $1$。

所以从后往前找到第一个非零的位置，后面的零全都删掉，前面保留，个数即为答案。

### Code

~~~cpp
#include<bits/stdc++.h>
using namespace std;
using LL=long long;
string s;
LL n,ans,t,loc;
int main(){
    cin>>t;
    while(t--){
        ans=0;
        cin>>s;
        n=s.size();
        s=' '+s;
        for(int i=n;i>=1;i--){
            if(s[i]>'0'){
                loc=i;
                break;
            }
        }
        for(int i=1;i<loc;i++)if(s[i]>'0') ans++;
        cout<<n-loc+ans<<endl;
    }
    return 0;
}
~~~