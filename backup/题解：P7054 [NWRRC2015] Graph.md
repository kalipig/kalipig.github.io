## 题解：P7054 [NWRRC2015] Graph
观察题目，我们要让**字典序最小**，就是要让第一位最小，然后第二位，以此类推，则可以想到贪心。\
连 $k$ 条边，就是要在拓扑排序中有 $k$ 步可以选择，且每一步找一个小一点的点来连。那么这个点怎么连呢？
这里参考了 @DengDuck 大佬的思路，直接把所有这样的点用一个大根堆存起来，在拓扑的过程中取出来用。\
同时，要字典序最小，需要把拓扑排序的队列换成小根堆，并且记录路径。

**注意：如果你的代码没过样例不着急**\
不要忘了有 
```special judge```，对于第一组样例，这个输出是可以的（~~被耽误了很久~~）：

```
5 1 4 2 3
2
5 1
2 3
```
### Code

```
#include<bits/stdc++.h>
#define endl '\n'
using namespace std;
using LL=long long;
LL n,m,in[500005],k,tmp[500005],cnt;
vector<LL> v[500005],order;
priority_queue<LL,vector<LL>,greater<LL> > q;
priority_queue<LL,vector<LL>,less<LL> > p;
vector<pair<LL,LL> > res;
void topsort(){
    for(int i=1;i<=n;i++) if(!in[i]) q.push(i);
    while((q.size()+p.size())){
        cnt++;tmp[cnt]=tmp[cnt-1];
        while(min(k,LL(q.size()))){
            if(q.top()>(!p.empty() ? p.top() : 0)&&q.size()==1) break;
            p.push(q.top()),q.pop(),k--;
        }
        if(!q.empty()){
            tmp[cnt]=q.top(),q.pop();
        } else{
            tmp[cnt]=p.top(),p.pop();
            if(tmp[cnt-1]) res.push_back({tmp[cnt-1],tmp[cnt]});
        }
        for(auto i : v[tmp[cnt]]){
            in[i]--;
            if(!in[i]) q.push(i);
        }
        order.push_back(tmp[cnt]);
    }
    return ;
}
int main(){
    cin>>n>>m>>k;
    for(int i=1,x,y;i<=m;i++){
        cin>>x>>y;
        v[x].push_back(y);
        in[y]++;
    }
    topsort();
    for(auto i : order){
        cout<<i<<' ';
    }
    cout<<endl;
    cout<<res.size()<<endl;
    for(auto i : res){
        cout<<i.first<<' '<<i.second<<endl;
    }
    return 0;
}
```