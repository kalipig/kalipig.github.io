## CF2093D

### Solution

分制模版题，先处理第一个问题。定义搜索状态 $\operatorname{dfs}(x,y,l,mn,mx)$ 表示当前在 $(x,y)$ 这个位置，当前块的边长为 $l$，当前块内的最大和最小值 $mx,mn$。

初始从最大的那个也就是右下角开始。

~~~cpp
void query1(LL x,LL y,LL l,LL mn,LL mx){
    if(l==1){//找到
        cout<<mn<<endl;
        return ;
    }
    LL len=mx-mn+1;//方便处理
    if(x<=l/2&&y<=l/2){//左上角
        query1(x,y,l/2,mn,mn+len/4-1);
    }
    if(x>l/2&&y>l/2){//右下角
        query1(x-l/2,y-l/2,l/2,mn+len/4,mn+len/2-1);
    }
    if(x>l/2&&y<=l/2){//左下角
        query1(x-l/2,y,l/2,mn+len/2,mn+len/4*3-1);
    }
    if(x<=l/2&&y>l/2){//右上角
        query1(x,y-l/2,l/2,mn+len/4*3,mx);
    }
}
~~~

在第二个问题中只需要加上当前查找的值 $d$ 即可。

从左上角开始，找范围。

~~~cpp
void query2(LL x,LL y,LL l,LL d,LL mn,LL mx){
    if(l==1){
        cout<<x<<' '<<y<<endl;
        return ;
    }
    LL len=mx-mn+1;
    if(d<=mn+len/4-1){
        query2(x,y,l/2,d,mn,mn+len/4-1);
    }
    if(d>=mn+len/4&&d<=mn+len/2-1){
        query2(x+l/2,y+l/2,l/2,d,mn+len/4,mn+len/2-1);
    }
    if(d>=mn+len/2&&d<=mn+len/4*3-1){
        query2(x+l/2,y,l/2,d,mn+len/2,mn+len/4*3-1);
    }
    if(d>=mn+len/4*3&&d<=mx){
        query2(x,y+l/2,l/2,d,mn+len/4*3,mx);
    }
}
~~~



### Code

~~~cpp
#include<bits/stdc++.h>
using namespace std;
using LL=long long;
LL n,q,t,pw[100]={1};
void query1(LL x,LL y,LL l,LL mn,LL mx){
    if(l==1){
        cout<<mn<<endl;
        return ;
    }
    LL len=mx-mn+1;
    if(x<=l/2&&y<=l/2){
        query1(x,y,l/2,mn,mn+len/4-1);
    }
    if(x>l/2&&y>l/2){
        query1(x-l/2,y-l/2,l/2,mn+len/4,mn+len/2-1);
    }
    if(x>l/2&&y<=l/2){
        query1(x-l/2,y,l/2,mn+len/2,mn+len/4*3-1);
    }
    if(x<=l/2&&y>l/2){
        query1(x,y-l/2,l/2,mn+len/4*3,mx);
    }
}
void query2(LL x,LL y,LL l,LL d,LL mn,LL mx){
    //cout<<x<<' '<<y<<endl;
    if(l==1){
        cout<<x<<' '<<y<<endl;
        return ;
    }
    LL len=mx-mn+1;
    if(d<=mn+len/4-1){
        query2(x,y,l/2,d,mn,mn+len/4-1);
    }
    if(d>=mn+len/4&&d<=mn+len/2-1){
        query2(x+l/2,y+l/2,l/2,d,mn+len/4,mn+len/2-1);
    }
    if(d>=mn+len/2&&d<=mn+len/4*3-1){
        query2(x+l/2,y,l/2,d,mn+len/2,mn+len/4*3-1);
    }
    if(d>=mn+len/4*3&&d<=mx){
        query2(x,y+l/2,l/2,d,mn+len/4*3,mx);
    }
}
int main(){
    for(int i=1;i<=60;i++) pw[i]=pw[i-1]*2;
    cin>>t;
    while(t--){
        cin>>n>>q;
        while(q--){
            LL x,y;char c1,c2;
            cin>>c1>>c2;
            if(c1=='-'){
                cin>>x>>y;
                query1(x,y,pw[n],1,pw[n*2]);
            }else {
                cin>>x;
                query2(1,1,pw[n],x,1,pw[n*2]);
            }
        }
    }
    return 0;
}
~~~