# CF241E 

### 题意
给你一张图，单向边且边权为 1，现在需要修改一些边的边权为 2 且保证 1 到 $n$ 的所有路径的长度相同。

### Solution
观察条件，可以发现要满足 1 到 $n$ 的所有路径的长度相同，就要使 1 到 $n$ 的路径上所有的点的距离相同。

所以对于一条边，有差分约束的条件:
$$1\leq dis[v]-dis[u] \leq 2$$
当然，对于不在 1 到 $n$ 的路径的点，我们不需要考虑。

先预处理一下从 $1\leftarrow n$ 和 $n\rightarrow1$ 的边，标记后重新建图，最后答案就为 $dis[v]-dis[u]$，否则在输出时可以直接输出 1。

关于 spfa 的差分约束具体操作，就是我们在建图的时候正着建一条边权为 2 的，反着建一条边权为 -1 的，最后就是判断一下负环（详见代码）。
### Code
~~~cpp
#include<bits/stdc++.h>
#define pi pair<LL,LL>
#define val first
#define id second
using namespace std;
using LL=long long;
LL n,m,U[5005],V[5005],dis[5005],tmp[5005],cnt[5005];
bool vis[5005];
vector<pi> g[5005];
inline void add(LL u,LL v,LL w){
    g[u].push_back({w,v});
}
void dfs1(LL x){
    tmp[x]++;
    if(x==n) return ;
    for(auto i : g[x]){
        if(!tmp[i.id]){
            dfs1(i.id);
        }
    }
}
void dfs2(LL x){
    tmp[x]++;
    if(x==1) return ;
    for(auto i : g[x]){
        if(tmp[i.id]==1){
            dfs2(i.id);
        }
    }
}
queue<LL> q;
bool spfa(LL s){
    memset(dis,0x3f,sizeof(dis));
    dis[s]=0,vis[s]=1;
    q.push(s);
    while(!q.empty()){
        LL to=q.front();q.pop();
        if(++cnt[to]>m) return 0;
        vis[to]=0;
        for(auto i : g[to]){
            if(dis[i.id]>dis[to]+i.val){
                dis[i.id]=dis[to]+i.val;
                if(!vis[i.id]){
                    vis[i.id]=1;
                    q.push(i.id);
                }
            }
        }
    }
    return 1;
}
int main(){
    ios::sync_with_stdio(false);cin.tie(nullptr),cout.tie(nullptr);
    cin>>n>>m;
    for(int i=1;i<=m;i++){
        cin>>U[i]>>V[i];
        add(U[i],V[i],1);
    }
    dfs1(1);
    for(int i=1;i<=n;i++) g[i].clear();
    for(int i=1;i<=m;i++) add(V[i],U[i],1);
    dfs2(n);
    if(tmp[n]<2){
        cout<<"No";return 0;//n不联通
    }
    //建新图
    for(int i=1;i<=n;i++) g[i].clear();
    for(int i=1;i<=m;i++){
        if(tmp[U[i]]==2&&tmp[V[i]]==2){
            add(U[i],V[i],2);
            add(V[i],U[i],-1);
        }
    }
    if(spfa(1)){
        cout<<"Yes\n";
        for(int i=1;i<=m;i++){
            if(tmp[U[i]]==2&&tmp[V[i]]==2){
                cout<<abs(dis[V[i]]-dis[U[i]])<<endl;
            }else cout<<"1\n";
        }
        return 0;
    }
    cout<<"No";
    return 0;
}
~~~