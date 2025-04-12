## CF2072D
### Problem
给你一个数列，长度为 $n$，现在移动一个位置的数，问让移动后数列中逆序对的数量最少的方法。
### Solution
我们首先可以想到就是要让减少的逆序对最多，然后可以发现题目中有一句话：

It is guaranteed that the sum of $n^2$ across all test cases does not exceed $4⋅10^6$.

意思就是用 $O(n^2)$ 的算法可以过，实际上存在复杂度更优的解法（数据结构维护）。
那么显然想到先枚举一个数 $a_i$，然后枚举目标位置 $j$，那么怎么计算减少的数量呢？
考虑记录一下：

- $cnt$:$a_j>a_i$ 的 $j$ 的数量。
- $cntt$:$a_j=a_i$ 的 $j$ 的数量。

所以最后减少的就是：
$$cnt-(i-j-cnt-cntt)$$
$$2\times cnt+cntt-(j-i)$$

### Code

```cpp
#include<bits/stdc++.h>
using namespace std;
using LL=long long;
LL n,a[500005],ans,l,r,t;
int main(){
    cin>>t;
    while(t--){
        ans=0;
        cin>>n;
        for(int i=1;i<=n;i++){
            cin>>a[i];
        }
        for(int i=1;i<=n;i++){
            LL cnt=0,cntt=0;
            for(int j=i+1;j<=n;j++){
                if(a[j]==a[i]) cntt++;
                if(a[j]<a[i]){
                    cnt++;
                    if(ans<=2*cnt+cntt-(j-i)){
                        ans=2*cnt+cntt-(j-i);
                        l=i,r=j;
                        //cout<<ans<<l<<' '<<r<<endl;
                    }
                }
            }
        }
        if(ans==0) l=r=1;
        cout<<l<<' '<<r<<endl;
    }
    return 0;
}
```