---
title: Generating Function
date: 2023-04-30
tag: [GF]
category: [Math]
math: true
---

生成函数入门

先来看这道题 [HDU2082](http://acm.hdu.edu.cn/showproblem.php?pid=2082)。

有 $26$ 种字母，第 $i$ 个字母权值为 $i$，有 $a_i$ 个。问有多少种方案可以构造出总价值介于 $[1,50]$ 的字母集合。

显然这题是可以 dp 的，但是我们换一种视角来看这个问题。

对于第 $k$ 个字母，我们最少一个都不选，最多选 $a_k$ 个。于是构造这样一个多项式：

$$
1+x^{k}+x^{2k}+\cdots +x^{a_k \cdot k}
$$

这里我们并不关心多项式的值，只是采用这个形式，用每一项的指数来表示权值，用系数表示方案的数量。

那么所有选择的方案，就是将这些多项式乘起来，即

$$
\prod_{i=1}^{26}\sum_{j=0}^{a_i}x^{ij}
$$

只需统计 $x^1,x^2,\cdots,x^{50}$ 的系数之和，就是所求的答案了。

``` cpp
#include <bits/stdc++.h>
using std::cin;
using std::cout;
using std::endl;
using std::vector;
using std::string;
using ll = long long;
using uint = unsigned int;
using ull = unsigned long long;
using pii = std::pair<int, int>;
const int inf = 0x3f3f3f3f;
const int maxn = 3e5 + 5;

void solve() {
    vector<ll> a(60);
    a[0] = 1;
    for (int i = 1; i <= 26; ++i) {
        int x;
        cin >> x;
        if (x == 0) continue;
        vector<ll> b(60);
        for (int j = 0; j <= 50; ++j) {
            for (int k = 0; k <= x; ++k) {
                if (i * k + j > 50) 
                    break;
                b[i * k + j] += a[j];
            }
        }
        a = b;
    }
    ll ans = std::accumulate(a.begin() + 1, a.begin() + 50 + 1, 0LL);
    cout << ans << endl;
}

int main() {
    std::ios::sync_with_stdio(false);
    cin.tie(nullptr);
    int T = 1;
    cin >> T;
    for (int t = 1; t <= T; ++t) {
        solve();
    }
    return 0;
}
```

使用同样的做法可以解决 [HDU1028](https://acm.hdu.edu.cn/showproblem.php?pid=1028)，把一个整数划分为若干个整数之和，不考虑顺序，求所有方案的数量。复杂度 $O(n^2\log n)$。

``` cpp
void solve(int n) {
    vector<ll> a(n + 1);
    a[0] = 1;
    for (int i = 1; i <= n; ++i) {
        vector<ll> b(n + 1);
        int m = n / i;
        for (int j = 0; j <= n; ++j) {
            for (int k = 0; k <= m; ++k) {
                if (i * k + j > n)
                    break;
                b[i * k + j] += a[j];
            }
        }
        a = b;
    }
    cout << a[n] << '\n';
}
```

以上就是生成函数最简单的应用。由于每个多项式中的自变量 $x$ 并没有什么意义，这种多项式也称为形式幂级数。

上面的形如 $\sum a_ix^i$ 的生成函数称为普通生成函数，可以解决组合问题。

另一类生成函数可以解决排列问题。


