## ABC428C
### Solution
括号序列用栈写永远是最方便的。\
把左右括号当作 $1,-1$。那么一个序列是好的一定满足 $\sum s_i=0$ 且 $\min(\sum_{j=1}^{i} s_j) \ge 0$。\
意思就是左右括号的数量要相同，且不能出现右括号在左括号前面的情况。

所以用栈维护这个和和和的最小值，只要满足条件就好了。

### Code
```cpp
#include<bits/stdc++.h>
using namespace std;
int main() {
    int k = 0,mn = 0,q;
    cin >> q;
    vector<pair<int, int>> g;
    for (int i = 1, op; i <= q; i++) {
        cin >> op;
        if (op == 1) {
            char c;
            cin >> c;
            g.push_back({k, mn});
            if (c == '(') {
                k++;
            } else {
                k--;
            }
            mn = min(mn, k);
            
        } else {
            auto tmp = g.back();
            g.pop_back();
            k = tmp.first;
            mn = tmp.second;
        }
        if (k == 0 && mn >= 0) {
            cout << "Yes\n";
        } else {
            cout << "No\n";
        }
    }
    return 0;
}
```