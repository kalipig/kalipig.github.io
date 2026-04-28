## CF2132D

### Solution
对于第 $k$ 个数字，我们首先确定是多少位数和哪一个数。\
然后把数位比它小的数字加起来，
再算比它小但是数位相同的数字。\
最后算它自己。\
计算位数和的原理就是先算有多少个，比如 $100\rarr 999$ 也就是三位数有 $9\times100$ 个，然后每个数字 $1\rarr9$ 会出现 $3\times 9$ 次，总和就是 $3\times45$，这里的 $45$ 就是数字 $1\rarr9$ 的和，下面代码里也是这个意思。

### Code
```cpp
using LL=long long;
LL calc(LL n) {//计算 n 位数的和
    if (n <= 0) return 0;
    if (n < 10) return n * (n + 1) / 2; //小于10直接输出，为什么去看后面
    LL base = 1;
    int len = 0;
    while (base * 10 <= n) {//确定位数
        base *= 10;
        len++;
    }
    LL first = n / base;//首位
    LL  r = n % base;//剩下的数字
    LL len = (LL)len * 45 * (base / 10);//其实是 len*9/2*base但是因为base是10的倍数所以可以这样防止小数
    LL p1 = (LL)first * (first - 1) / 2 * base + first * len;//
    LL p2 = first * ( r + 1) + calc( r);//答案就是当前位的加上小一位的
    return p1 + p2;
}
void solve(){
    LL k;
    cin >> k;
    int d = 1;
    LL base = 1;
    while (k > 9 * base * d) {
        k -= 9 * base * d;
        d++;
        base *= 10;
    }
    LL  st = base;//确定位数
    LL index = (k - 1) / d;//首位
    LL  r = (k - 1) % d + 1;//包含第 k 位的数剩下的位数
    LL num =  st + index;//包含第 k 位剩下的数字
    LL p1 = calc( st - 1);//计算位数小的那个部分
    LL p2 = calc( st + index - 1) - calc( st - 1);//和自己数位相同的减去大的部分
    string s = to_string(num);//快速计算位数和
    LL p3 = 0;
    for (int i = 0; i <  r; i++) {
        p3 += s[i] - '0';
    }
    cout << p1 + p2 + p3 << endl;//把三个部分加起来
}
```