## CF706E
### Solution
这道题感觉可以用差分或者线段树吧，但是没写出来，看了题解是链表。\
这是我第一次写链表，感觉挺不错的。

把两个矩形交换，这里可以感性理解为一个网，要把两块网剪下来交换，就是把和四周连接的线剪断，然后重新连上。\
这里的这个线就是链表的指针，把它交换一下就好了。

就是把链表初始化，然后每次询问围着矩形一圈交换一下两个矩形边界的指针就好了。

### Code
```cpp
#include<bits/stdc++.h>
using namespace std;
#define y1 yy
using LL=long long;
struct node{
	LL val;
	node *down,*right;
};
node *a[1005][1005];
LL n,m,q,x1,x2,y1,y2,l,c;

int main(){
	ios::sync_with_stdio(false);
	cin.tie(nullptr);
	
	cin>>n>>m>>q;
	for(int i=0;i<=n+1;i++) 
		for(int j=0;j<=m+1;j++) 
			a[i][j]=new node();
	for(int i=1;i<=n;i++){
		for(int j=1;j<=m;j++){
			int x;
			cin>>x;
			a[i][j]->val=x;
		}
	}
	for(int i=0;i<=n;i++){
		for(int j=0;j<=m;j++){
			a[i][j]->down=a[i+1][j];
			a[i][j]->right=a[i][j+1];
		}
	}
	
	while(q--){
		cin>>x1>>y1>>x2>>y2>>l>>c;
		node *p1=a[0][0],*p2=a[0][0];
		
		for(int i=1;i<x1;i++) p1=p1->down;
		for(int i=1;i<y1;i++) p1=p1->right;
		
		for(int i=1;i<x2;i++) p2=p2->down;
		for(int i=1;i<y2;i++) p2=p2->right;
		
		node *u1=p1,*u2=p2;
		for(int i=1;i<=c;i++){
			u1=u1->right;
			u2=u2->right;
			swap(u1->down,u2->down);
		}
		for(int i=1;i<=l;i++){
			u1=u1->down;
			u2=u2->down;
			swap(u1->right,u2->right);
		}
		u1=p1,u2=p2;
		for(int i=1;i<=l;i++){
			u1=u1->down;
			u2=u2->down;
			swap(u1->right,u2->right);
		}
		for(int i=1;i<=c;i++){
			u1=u1->right;
			u2=u2->right;
			swap(u1->down,u2->down);
		}
	}
	node *x=a[0][1]->down;
	for(int i=1;i<=n;i++){
		node *y=x;
		for(int j=1;j<=m;j++){
			cout<<y->val<<' ';
			y=y->right;
		}
		cout<<"\n";
		x=x->down;
	}
	return 0;
}
```