## 题解：CF2060E Graph Composition
题意：**给两个图 $F$ 和 $G$，在 $F$ 中删边或加边，使两图连通性相同。**
### Solution
很容易想到使用并查集来维护连通性和加边，但是要删边的话，会影响整个 $father$ 数组中有关联的点，复杂度不能通过此题。\
所以我们反过来想，先在 $F$ 里加边，然后再算多出来的边。
### Solving
~~码风良好。~~
- $a,b$：$F$ 图中原有的边。
- $fa+f/g$：并查集。
- $h+f/g$：标记此点是否为联通分量的代表。
- $cnt1/2$：本来就有的边|需要删的边。

具体过程见注释。

```cpp
#include<bits/stdc++.h>
using namespace std;
using LL=long long;
LL t,n,m1,m2,ans,a[500005],b[500005],fag[500005],faf[500005],hg[500005],hf[500005],cnt1,cnt2;
LL findf(LL x){
    if(faf[x]==x) return x;
    return faf[x]=findf(faf[x]);
}
LL findg(LL x){
    if(fag[x]==x) return x;
    return fag[x]=findg(fag[x]);
}
int main(){
    cin>>t;
    while(t--){
        cin>>n>>m1>>m2;
        ans=cnt1=cnt2=0;
        for(int i=1;i<=n;i++){
            faf[i]=fag[i]=i;
            hf[i]=hg[i]=0;
        }
        for(int i=1;i<=m1;i++){
            cin>>a[i]>>b[i];
        }
        for(int x,y,i=1;i<=m2;i++){
            cin>>x>>y;
            fag[findg(x)]=findg(y);//建立并查集
        }
        for(int i=1;i<=n;i++){
            if(!hg[findg(i)]){
                hg[findg(i)]=1,cnt1++; 处理标记
            }
        }
        for(int i=1;i<=m1;i++){
            if(fag[a[i]]==fag[b[i]]){
                faf[findf(a[i])]=findf(b[i]); //需要加的边
            }else ans++;
        }
        for(int i=1;i<=n;i++){
            if(!hf[findf(i)]){
                hf[findf(i)]=1; //需要删的边
                cnt2++;
            }
        }
        cout<<ans+cnt2-cnt1<<endl; //本来就有的边不需要操作
    }
    return 0;
}
```