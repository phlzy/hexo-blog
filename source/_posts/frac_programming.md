---
title: 分数规划
date: 2023-04-06
tag: [algorithm]
category: [Algorithm and DataStructure]
math: true
---

分数规划问题一般表现为求一个分式的极值。

``` cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    int n, W;
    cin >> n >> W;
    vector<int> a(n + 1);
    vector<int> b(n + 1);
    double l = 0, r = 0;
    for (int i = 1; i <= n; ++i) {
        cin >> b[i] >> a[i];
        r = max(r, 1.0 * a[i] / b[i]);
    }
    double ans;
    vector<double> dp(W + 1, 0);
    auto check = [&](double mid) -> bool {
        for (int i = 1; i <= W; ++i)
            dp[i] = -1e9;
        for (int i = 1; i <= n; ++i) {
            for (int j = W; j >= 0; j--) {
                int k = min(W, j + b[i]);
                dp[k] = max(dp[k], dp[j] + a[i] - mid * b[i]);
            }
        }
        return dp[W] > 0;
    };
    for (int T = 1; T <= 100; ++T) {
        double mid = (l + r) / 2;
        if (check(mid))
            ans = mid, l = mid;
        else
            r = mid;
        if (fabs(r - l) < 1e-6)
            break;
    }
    cout << (int) (ans * 1000) << endl;
}
```