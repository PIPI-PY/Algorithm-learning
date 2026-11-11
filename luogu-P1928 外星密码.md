# 题解：P1928 外星密码

## 题目大意

给定一个压缩字符串，压缩规则为：连续的若干相同子串 `X` 会被压缩为 `[DX]` ，其中 `D` 是 $1\sim99$ 的整数，表示重复次数。压缩可以嵌套（如 `[2[2CB]]` ）。要求还原出完整的解压字符串。

---

## 核心思路

> ```
> 遇到普通字符 → 直接加入结果
> 遇到 '[' → 读出重复次数 num，递归解压后续内容，重复 num 次
> 遇到 ']' → 当前层解压结束，返回
> ```

**解释：**

用递归处理嵌套结构。`gety()` 函数从当前位置开始解压，直到遇到 `]` 或字符串结束：

- 普通字母：直接拼接到 `s` ；
- `[` ：先读数字 `num` ，再递归调用 `gety()` 得到内层解压结果 `s2` ，把 `s2` 重复 `num` 次拼到 `s` ；
- `]` ：说明当前层结束，`i++` 跳过并返回 `s` 。

**形象理解：** 把 `[` 看作 `+` 、数字与内容之间看作 `(` 、`]` 看作 `)` ：

```
SAD[2RE[2WA]]  →  SAD + 2(RE + (2(WA)))
```

## 递归过程示例

以 `SAD[2RE[2WA]]` 为例：

1. 读入 `S` 、`A` 、`D` ，直接拼入 `s` ；
2. 遇 `[` ，读 `num=2` ，递归解压 `RE[2WA]]` ：
   - 拼入 `R` 、`E` ；
   - 遇 `[` ，读 `num=2` ，递归解压 `WA]]` → 返回 `WA` ；
   - 重复 2 次得 `WAWA` ，与 `RE` 合并得 `REWAWA` ；
   - 遇 `]` 返回 `REWAWA` ；
3. 外层重复 2 次得 `REWAWAREWAWA` ；
4. 与 `SAD` 合并，最终输出 `SADREWAWAREWAWA` 。

---

## 代码实现

```cpp
#include <bits/stdc++.h>
using namespace std;
#define bearcookies ios::sync_with_stdio(false), cin.tie(nullptr), cout.tie(nullptr)
typedef long long ll;

string gety();

int i = 0;
string s1;

int main()
{
    bearcookies;

    // freopen("text.in", "r", stdin);
    // freopen("text.out", "w", stdout);

    cin >> s1;
    cout << gety() << "\n";
    return 0;
}

string gety()
{
    string s = "";
    while(i < s1.size()){
        if (s1[i] == '['){
            i++;
            int num = 0;
            while (s1[i] >= '0' && s1[i] <= '9'){
                num = 10 * num + s1[i] - 48;
                i++;
            }
            string s2 = gety();
            while (num--){
                s += s2;
            }
        }else if (s1[i] == ']'){
            i++;
            return s;
        }else{
            s += s1[i];
            i++;
        }
    }
    return s;
}
```

---