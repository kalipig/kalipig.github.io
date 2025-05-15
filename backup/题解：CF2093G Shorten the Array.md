## CF2093G

### Solution

参考了上面大佬的做法。

看到子串，想到双指针。因为求任意两个数异或的最大值是字典数的基本操作，比如[这道题](https://loj.ac/p/10050)就是一个 01 字典树的例子。

考虑做法：

- 每次把一个 $a_i$ 加入字典树。
- 记录一个左端点 $j$，每次加入时如果存在一个 $x$ 使 $(a_i \bigoplus x)$ 就把 $j$ 往右移，并且删掉 $a_j$。
- 在移动的过程中记录 $ans=\min(i-j+1)$。

可以发现如果 $a_j=x$ 后删掉 $a_j$ 就不能往前移了，而在删掉 $a_j$ 之后即使还有符合的也已经大于 $ans$，可以不要考虑。



### Code

~~~cpp
#include<bits/stdc++.h>
using namespace std;
using LL=long long;
LL n,t,k,a[500005],ans;
struct trie{
	LL num=1,tr[5000005][2],cnt[5000005];
	inline LL node(){
		LL x=++num;
		tr[x][1]=tr[x][0]=cnt[x]=0;
		return x;
	}
	LL query(LL x){
		LL loc=1,res=0;
		for(int i=32;i>=0;i--){
			LL p=(x>>i)&1;
			if(cnt[tr[loc][p^1]]){
				p=p^1;
			}
			loc=tr[loc][p];
			res|=((p)<<i);
		}
		return res^x;
	}
	void add(LL x){
		LL loc= 1;
        for(int i = 32; i >= 0; i--) {
            LL p = (x >> i) & 1;
            if(!tr[loc][p]) {
                tr[loc][p] = node();
            }
            loc = tr[loc][p];
            cnt[loc]+=1;
        }
	}
	void del(LL x){
		LL loc= 1;
        for(int i = 32; i >= 0; i--) {
            LL p = (x >> i) & 1;
            if(!tr[loc][p]) {
                tr[loc][p] = node();
            }
            loc = tr[loc][p];
            cnt[loc]-=1;
        }
	}
}tree;
int main(){
	cin>>t;
	while(t--){
		ans=1e12;
		cin>>n>>k;
		for(int i=1;i<=n;i++) cin>>a[i];
		if(k==0){
			cout<<"1\n";
			continue;
		}
		
		tree.num=0,tree.node();
		for(int i=1,j=1;i<=n;i++){
			while(i>=j&&tree.query(a[i])>=k){
				ans=min(ans,LL(i-j+1));
				tree.del(a[j++]);
			}
			tree.add(a[i]);
		}
		cout<<(ans<=n ? ans : -1)<<endl;
	}
	return 0;
}


~~~