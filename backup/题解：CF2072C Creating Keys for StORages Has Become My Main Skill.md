## CF2072C
### Problem
一个长度为 $n$ 的数组 $a$，满足以下条件：
- $a_1 \mid a_2 \mid a_3\dots a_n=x$，其中$ a|b $是数字 $a$ 和 $b$ 的按位“或”。
- ${MEX}({a_1,a_2,a_3\dots a_n})$ 在所有这样的数组中最大化。

${MEX}(S)$ 是最小的非负整数 $z$，使得 $z$ 不包含在集合 $S$ 中，并且所有 $0<y<z$ 都包含在 $S$ 中。 

### Solution
因为要让整个序列或的结果为 $x$，最好的是让整个序列都为 $x$。\
然后看第二个条件，我们可以发现要在不影响结果为 $x$ 的情况下让 ${MEX}$ 最大。\
怎么才能不影响结果，因为按位或的性质是某一位上只要有 1，那么最后结果一定有 1，所以可以由此找到最大的 ${MEX}$，并且不会有任何一位有影响结果的 1。
~~~cpp
for(mex=0;mex<=n-1;mex++){
    if((mex&x)!=mex) break;
}
~~~
然后就输出 $1\rightarrow {{MEX}-1}$，多的就全部输出 $x$。

### Code
~~~cpp
#include<bits/stdc++.h>
using namespace std;
using LL=long long;
LL n,x,t,mex;
int main(){
    cin>>t;
    while(t--){
        LL tmp=0;
        cin>>n>>x;
        for(mex=0;mex<=n-1;mex++){
            if((mex&x)!=mex) break;
            tmp|=mex;
        }
        if(mex==n&&tmp!=x) mex--;
        for(int i=0;i<mex;i++){
            cout<<i<<' ';
        }
        for(int i=mex+1;i<=n;i++) cout<<x<<' ';
        cout<<endl;
    }
    return 0;
}
~~~