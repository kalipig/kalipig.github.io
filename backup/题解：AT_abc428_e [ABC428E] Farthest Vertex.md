## ABC428E
### Solution 
这题放第五题是真的简单了。\
现在考虑四个点，$v,s,t,u$，这里 $v$ 是题目中的，$s,t$ 分别是树的直径的起点和终点，$u$ 是任意一点且 $u\neq s,u\neq t$。\
分类讨论一下：
- 如果 $v$ 在直径上，那么答案就是 $\max(dis(v,t),dis(v,t))$，如果存在 $u$ 是更长的，那么把 $v\rarr u$ 和直径的另一半拼起来一定比直径长，故不存在。
- 如果 $v$ 不在直径上，那么一定要先走到直径上，再以刚刚的情况考虑，如果不走到直径上而是走到 $u$，也就是说 $v\rarr u$ 的长度会大于 $\max(dis(x,t),dis(x,t))+dis(x,v)$ 其中 $x$ 是直径上一点，那么直径应该就是 $u\rarr t$ 或者 $u\rarr s$，故不成立。

意思就是一定要先走到直径上再走到端点上，上面只是严格的证明。\
记得选编号大的。

### Code
```cpp
#include<bits/stdc++.h>
using namespace std;
const int N=5e5+10;
int n,a,b,u1,u2;
int d[N],d1[N],d2[N];
vector<int> g[N];
queue<int> q;
void bfs(int s,int dis[]){
    memset(dis,-1,sizeof(d));
    dis[s]=0;
    while(!q.empty())q.pop();
    q.push(s);
    while(!q.empty()){
        int u=q.front();q.pop();
        for(int v:g[u])if(dis[v]==-1)dis[v]=dis[u]+1,q.push(v);
    }
}
int main(){
    cin>>n;
    for(int i=1;i<n;i++){
        cin>>a>>b;
        g[a].push_back(b);
        g[b].push_back(a);
    }
    bfs(1,d);
    u1=1;
    for(int i=2;i<=n;i++)if(d[i]>d[u1]||(d[i]==d[u1]&&i>u1))u1=i;
    bfs(u1,d1);
    u2=u1;
    for(int i=1;i<=n;i++)if(d1[i]>d1[u2]||(d1[i]==d1[u2]&&i>u2))u2=i;
    bfs(u2,d2);
    for(int i=1;i<=n;i++){
        if(d1[i]>d2[i])cout<<u1<<"\n";
        else if(d1[i]<d2[i])cout<<u2<<"\n";
        else cout<<max(u1,u2)<<"\n";
    }
    return 0;
}

```