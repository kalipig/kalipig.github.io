## CF2160B
### Solution
因为 $b$ 是前缀的一个出现次数，所以先对 $b$ 作差分，设 $x=b_i-b_{i-1}$。
接下来就是三种情况：
- $x>i$，这个时候不可能，所以输出 $-1$。
- $x=i$，这个时候说明出现了新的数，$a_i=k+1,k=k+1$，这里 $k$ 表示的是出现的数的种类个数。
- $x<i$，这说明前 $i$ 个数只出现了 $x$ 个，那么 $a_i=a_{i-x}$，这样后面都是一样的。其实也不一定这 $i-x$ 个数都一样，但是至少没有出现新的数。

然后就输出就好了。

### Code
```cpp
#include<bits/stdc++.h>
using namespace std;
using LL=long long;
LL n;
void solve(){
    cin >> n;
    vector<LL> b(n+5),a(n+5);
    for (int i = 1; i <= n; i++) {
        cin >> b[i];
    }
    a[1] = 1;
    LL k=2;
    bool flag = true;
    for (int i = 2; i <= n; i++) {
        LL x= b[i] - b[i-1];
        if (x > i) {
            flag = false;
            break;
        }
        if (x == i) {
            a[i] = k++;
        } else {
            LL tmp= i - x;
            if (tmp <= 0) {
                flag = false;
                break;
            }
            a[i] = a[tmp];
        }
    }
    if (!flag) {
        cout << "-1\n";
    } else {
        for (int i = 1; i <= n; i++) {
            cout << a[i] <<" ";
        }
    }
    cout<<"\n";
}
int main(){
    int t;
    cin>>t;
    while(t--) solve();
    return 0;
}
```