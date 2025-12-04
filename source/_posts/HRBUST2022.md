---
title: 2022 HRBUST Summer Trainning
date: 2022-08-08
tag: [contest]
category: [Competitive Programming]
math: true
---

Mediocre problems. 

<!--more-->

[link](https://ac.nowcoder.com/acm/contest/37895)

# A

统计一下连续段，然后前缀和处理即可。

``` cpp
#include <bits/stdc++.h>
using namespace std;
using ll = long long;
#define endl '\n'
#define pb push_back
void solve() {
    string s;
    cin >> s;
    vector<ll> a;
    int cnt = 0;
    for (auto &&c : s) {
        if (c == '1')
            cnt++;
        else if (cnt)
            a.pb(cnt), cnt = 0;
    }
    if (cnt)
        a.pb(cnt);
    for (auto &&i : a)
        i = i * i;
    int n = a.size();
    vector<ll> p0(n + 1), p1(n + 1);
    for (int i = 0; i < n; ++i) {
        if (i & 1) {
            p0[i + 1] = -a[i];
            p1[i + 1] = a[i];
        } else {
            p0[i + 1] = a[i];
            p1[i + 1] = -a[i];
        }
        p0[i + 1] += p0[i];
        p1[i + 1] += p1[i];
    }
    ll mxp0 = p0[n], mxp1 = p1[n];
    ll ans = 0;
    for (int i = n - 1; i >= 0; --i) {
        ans = max({ans, mxp0 - p0[i], mxp1 - p1[i]});
        mxp0 = max(mxp0, p0[i]);
        mxp1 = max(mxp1, p1[i]);
    }
    cout << ans << endl;
}
int main() {
    ios::sync_with_stdio(false);
    cin.tie(0), cout.tie(0);
    int T = 1;
    // cin >> T;
    while (T--)
        solve();
    return 0;
}
```

# B

容易发现后面几位是没用的，所以就相当于不能异或出 $0$。经典双指针。

``` cpp
#include <bits/stdc++.h>
using namespace std;
using ll = long long;
#define endl '\n'
#define pb push_back
void solve() {
    int n, k;
    cin >> n >> k;
    vector<int> a(n);
    for (auto &i : a) {
        cin >> i;
        i >>= k;
    }
    int ans = 0;
    unordered_map<int, int> mp;
    int l = 0, r = 0;
    for (; l < n; ++l) {
        while (r < n) {
            if (!mp[a[r]]) {
                mp[a[r]]++;
                r++;
            } else {
                break;
            }
        }
        ans = max(ans, r - l);
        mp[a[l]]--;
    }
    cout << ans << endl;
}
int main() {
    ios::sync_with_stdio(false);
    cin.tie(0), cout.tie(0);
    int T = 1;
    // cin >> T;
    while (T--)
        solve();
    return 0;
}
```

# C

比较经典的二分答案+树形dp。dp 的状态是以当前节点为根节点形成的连通块内两种可乐的 $4$ 种奇偶性的组合各自的最大值。细节比较多，懒得写了。

# D

暴力 BFS。懒得写了。

# E

大概是线段树上二分来模拟这个过程。懒得写了。

# F

当最大值为 $m$ 时，前面的 $n-1$ 个位置需要填入 $[0,m]$ 中的数，就相当于将 $n-1$ 个相同的球放进 $m+1$ 个不同的箱子里，允许出现空箱。

不允许出现空箱的时候可以用隔板法，就是在 $n-2$ 个空隙里选 $m$ 个位置插入隔板。那么允许空箱的时候就在每个箱子里加一个虚拟的球，就转化为了之前的问题，答案就是 $\binom{n+m-1}{m}$。

预处理组合数后枚举 $m$，复杂度 $O(n+k)$。

``` cpp
#include <bits/stdc++.h>
using namespace std;
using ll = long long;
#define endl '\n'
#define pb push_back
const ll mod = 1e9 + 7;
const int maxn = 2e6 + 5;
ll qpow(ll b, ll k) {
    ll ret = 1;
    while (k) {
        if (k & 1)
            ret = ret * b % mod;
        b = b * b % mod;
        k /= 2;
    }
    return ret;
}
ll pinv(ll x) { return qpow(x, mod - 2); }
ll fact[maxn], ifact[maxn];
void init_fact(int n) {
    fact[0] = 1;
    for (int i = 1; i <= n; ++i)
        fact[i] = fact[i - 1] * i % mod;
    ifact[n] = pinv(fact[n]);
    for (int i = n - 1; i >= 0; --i)
        ifact[i] = ifact[i + 1] * (i + 1) % mod;
}
ll C(int n, int m) { 
    return fact[n] * ifact[n - m] % mod * ifact[m] % mod; 
}
void solve() {
    ll n, k;
    cin >> n >> k;
    init_fact(n + k);
    ll ans = 0;
    for (int i = 1; i <= k; ++i) {
        ans += C(n + i - 1, i) * i % mod;
        ans %= mod;
    }
    cout << ans << endl;
}
int main() {
    ios::sync_with_stdio(false);
    cin.tie(0), cout.tie(0);
    int T = 1;
    // cin >> T;
    while (T--)
        solve();
    return 0;
}
```

# G

贪心。

``` cpp
#include <bits/stdc++.h>
using namespace std;
using ll = long long;
#define endl '\n'
void solve() {
    int n;
    cin >> n;
    vector<int> a(n);
    for (int i = 0; i < n; ++i)
        cin >> a[i];
    int l, r;
    cin >> l >> r;
    int ans = 0;
    for (auto &&i : a) {
        if (l <= i && i <= r)
            ans = max(ans, i);
    }
    cout << (ans + r) << endl;
}
int main() {
    ios::sync_with_stdio(false);
    cin.tie(0), cout.tie(0);
    int T = 1;
    // cin >> T;
    while (T--)
        solve();
    return 0;
}
```

# H

令 $p_i$ 表示第 $i$ 天结束的概率，$q_i=\prod_{j\lt i}(1-p_j)$。

先列出期望的式子：

$$
E= \sum_{i=1}^{\infty}p_iq_i i
$$

然后根据 $p_i$ 的周期性，每 $n$ 天分一段。令 $E_n=\sum_{i=1}^n p_iq_i i$，$P_n=\sum_{i=1}^n p_iq_i$：

$$
E= \sum_{i=0}^{\infty}q_n^i (E_n+inP_n)\\
(1-q_n)E=E_n+nP_n\sum_{i=1}^{\infty}q_n^i \\
E=\frac{E_n}{1-q_n}+\frac{nP_nq_n}{(1-q_n)^2}
$$

这样就可以 $O(n)$ 求出来了。

``` cpp
#include <bits/stdc++.h>
using namespace std;
using ll = long long;
#define endl '\n'
#define pb push_back
const ll mod = 1e9 + 7;
const int maxn = 1e5 + 5;
ll qpow(ll b, ll k) {
    ll ret = 1;
    while (k) {
        if (k & 1)
            ret = ret * b % mod;
        b = b * b % mod;
        k /= 2;
    }
    return ret;
}
ll pinv(ll x) { return qpow(x, mod - 2); }
ll a[maxn];
void solve() {
    ll n;
    cin >> n;
    ll inv100 = pinv(100);
    for (int i = 1; i <= n; ++i) {
        cin >> a[i];
        a[i] = a[i] * inv100 % mod;
    }
    ll q = 1, p = 0, E = 0;
    for (int i = 1; i <= n; ++i) {
        p = (p + q * a[i] % mod) % mod;
        E = (E + q * a[i] % mod * i % mod) % mod;
        q = q * (1 + mod - a[i]) % mod;
    }
    ll tmp = pinv((1 + mod - q) % mod);
    ll ans = E * tmp % mod + n * p % mod * q % mod * tmp % mod * tmp % mod;
    cout << ans % mod << endl;
}
int main() {
    ios::sync_with_stdio(false);
    cin.tie(0), cout.tie(0);
    int T = 1;
    // cin >> T;
    while (T--)
        solve();
    return 0;
}
```

感觉这种难度的题放在校赛或者个人训练赛还是很合适的。