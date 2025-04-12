## 题解：CF2060C Game of Mathletes
题意：给你一个数列，A 先选，B 再选，若选出来两数相加为 $k$，则答案加一，A 想要答案小，B 希望答案大，都采用最佳策略，问答案为多少。
### 策略
A 是先手，所以考虑他随便选了一个，此时看 B，他一定会尽量选可以相加为 $k$ 的，如果没有可以选的就可以选同样对答案没有贡献的。
### Result
答案是一组符合条件的数中数量少的那个然后相加。
### Code
~~码风良好~~

```cpp
#include<bits/stdc++.h>
using namespace std;
using LL=long long;
LL t,k,cnt,n,cn,sc,a[500005];
int main(){
	cin>>t;
	while(t--){
		memset(a,0,sizeof(a));
		sc=0;
		cin>>n>>k;
		for(int i=1;i<=n;i++){
			LL x;cin>>x;
			a[x]++;
		}
		for(int i=1;i<=k/2;i++){
			if(i+i==k){
				sc+=a[i]/2;
				continue;
			}
			sc+=min(a[i],a[k-i]);
		}
		cout<<sc<<endl;
	}
	return 0;
}
```