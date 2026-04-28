## CF2155D
### Solution
这种交互题，一般可以先试着按顺序问。\
比如现在知道 $1\rarr i-1$ 和 $1\rarr i$ 的答案 $res_1$ 和 $res_2$。\
如果 $res_1=res_2$，说明 $a_i$ 这个数是出现的第一次，否则就是第二次出现了 $a_i$，就记录一下。\
但是如果这个 $a_i$ 不够大可能不会显示出来，所以我们需要保证如果 $a_i$ 是第二次出现，那么后面的询问就不再问 $i$ 这个位置，这样就避免了大小的问题，因为不会出现其它的两个相同的数了。

这里已经是 $2\times n$ 次询问了，还有 $n$ 次，一般交互题是要把次数用完的哦。

但是还不知道每个数的第一次在哪里出现，就可以对于每个不是第二次出现的数的位置，一共 $n$ 个，询问一下，同时询问所有出现第二次出现的数的位置，一定会匹配一个，就可以找到了。

### Code
```cpp
#include<bits/stdc++.h>
using namespace std;
using LL=long long;
void solve(){
    LL n;
    cin >> n;
    vector<LL> res(n*2+5),app(n*2+5,0),second(n+5,0);
    LL num=1,la=0;
    for(int i=2;i<=n*2;i++){
        cout<<"? "<<num+1<<' ';
        for(int j=1;j<=i;j++){
            if(!app[j]) cout<<j<<' ';
        }
        cout<<"\n";
        cout.flush();
        LL x;
        cin>>x;
        if(x==la||x==0) num++;
        else app[i]=1,second[x]=i,res[i]=x;
        la=x;
    }
    num=0;
    for(int i=1;i<=n*2;i++){
        if(app[i]) continue;
        cout<<"? "<<n+1<<' ';
        for(int i=1;i<=n;i++) cout<<second[i]<<' ';
        cout<<i<<endl;
        LL x;
        cin>>x;
        res[i]=x;
        cout.flush();
    }
    cout << "!";
    for(int i = 1; i <= 2*n; i++){
        cout << " " << res[i];
    }
    cout << endl;
    cout.flush();
}

int main(){
    int t;
    cin >> t;
    while(t--) solve();
    return 0;
}
```