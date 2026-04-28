## ABC242D
### Problem
给定一个字符串 $S$。

设 $S_0=S$，令 $S_i$ 是同时替换 $S_{i-1}$ 中字符的结果，替换规则如下：$A\rightarrow BC$，$B\rightarrow CA$，$C\rightarrow AB$。

查询如下：打印 $S_t$ 的第 $k$ 个字符。

### Solution
这个 $10^{18}$ 的数据范围，我们考虑分治。\
定义 $\operatorname{dfs}(t,k)$ 表示 $S_t$ 的第 $k$ 个字符。
那么很明显有：
$$t=0\Longrightarrow s_k$$
$$k=0\Longrightarrow\space (s_1+t \bmod{3})\bmod{3}$$
然后考虑转移，因为每一次替换第 $k$ 的位置会映射到 $k\times{2}$，$k\times{2}+1$。\
所以有:
$$\operatorname{dfs}(t,k)\Longrightarrow \operatorname{dfs}(t-1,(k+1)\div{2}+1+(k\bmod{2}))$$

### Code
~~~cpp
#include<bits/stdc++.h>
using LL=long long;
using namespace std;
LL q,t,k,n;
string s;
LL dfs(LL t,LL k){
	if(t==0) return s[k]-'A';
	if(k==1) return (s[1]-'A'+t%3)%3;
    return (dfs(t-1,(k+1)/2)+1+(k%2))%3;
}
int main(){
	cin>>s;
    n=s.size();
	cin>>q;
	while(q--){
		cin >> t >> k; 
		cout << char(dfs(t,k)%3+'A') <<endl; 
	}
	return 0;
}
~~~