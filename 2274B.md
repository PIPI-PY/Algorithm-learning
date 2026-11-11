# 题解：2274/B


## 题目大意

给定数组 $a_1,\dots,a_n$。对 $i=2,3,\dots,n$ 依次执行：令 $s=a_1+\dots+a_{i-1}$（当前前缀和），把 $a_i$ 替换为 $a_i-s$、$a_i$、$a_i+s$ 三者之一。求所有选择下，最终 $\max(a_1,\dots,a_n)$ 的最大值。

---

## 核心推导

> ```
> ans = a[i] + |s|;
> |s|要取max，就要s > 0时，s取最大值;s < 0, s取最小值 
> s' = s + a[i] + {s, 0, -s}
> s' = {a[i] + 2 * s, a[i]}
> ```

**解释：**

第 $i$ 步 $a_i$ 被替换成三种值，其中最大的那个是 $a_i + |s|$（$s>0$ 时取 $a_i+s$，$s<0$ 时取 $a_i-s$）。所以要最大化答案，就要让 $|s|$ 尽量大——即 $s>0$ 时取前缀和最大值，$s<0$ 时取前缀和最小值。

而新的前缀和 $s'$ 为：

$$
s' = s + a_i + \{s,\;0,\;-s\} = \{a_i+2s,\;a_i\}
$$

即新的前缀和只可能是 $a_i$ 或 $a_i+2s$ 两个端点值。

---

## 代码实现

```cpp
#include <bits/stdc++.h>
using namespace std;
#define beercookies ios::sync_with_stdio(false), cin.tie(nullptr), cout.tie(nullptr)
typedef long long ll;

int main()
{
    beercookies;// freopen("text.out", "w", stdout);

    // freopen("text.in", "r", stdin);
    // freopen("text.out", "w", stdout);

    int n;
    cin >> n;
    while (n--) {
        int b;
        cin >> b;

        vector<ll> a(b + 1);
        ll maxidx = 0;
        ll minidx = 0;
        ll ans = -1000000000000000;
        for (int i = 1; i <= b; i++) {
            cin >> a[i];
            ans = max(ans, a[i] + max(maxidx, abs(minidx)));
            minidx = min(a[i] + 2 * minidx, a[i]);
            maxidx = max(a[i] + 2 * maxidx, a[i]);
        }
        cout << ans << "\n";
    }
   
```

---
