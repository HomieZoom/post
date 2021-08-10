---
title:【leetcod每日一题】2021-8-8/[1137. 第 N 个泰波那契数]
date:2021-8-8
tags:Leetcode
authoe:mili


---

## 【leetcod每日一题】2021-8-8[[1137. 第 N 个泰波那契数](https://leetcode-cn.com/problems/n-th-tribonacci-number/)]

### 题目：

泰波那契序列 Tn 定义如下： 

T0 = 0, T1 = 1, T2 = 1, 且在 n >= 0 的条件下 Tn+3 = Tn + Tn+1 + Tn+2

给你整数 n，请返回第 n 个泰波那契数 Tn 的值。

### 题解：

很简单的题目，力扣定位是简单题，确实也很简单。使用动态规划的思想，然后在使用滚动数组的办法亚索空间即可。

不说了，上代码：

```python
class Solution:
    def tribonacci(self, n: int) -> int:
        if n < 2:
            return n
        elif n == 2:
            return 1
        dp = [float("INF") for _ in range(n + 1)]
        dp[0] = 0
        dp[1] = 1
        dp[2] = 1

        for i in range(3, n + 1):
            dp[i] = dp[i -1] + dp[i - 2] + dp[i - 3]

        return dp[-1]
```

这里为了方便理解还是使用的动态规划的写法，没有进一步压缩空间。

```go
func tribonacci(n int) int {
    if n == 0 {
        return n
    }
    if n <= 2{
        return 1
    }
    p, q, r, s := 0, 0, 1, 1
    for i := 3; i <= n; i++{
        p = q
        q = r
        r = s
        s = p + q + r
    } 
    return s
}
```

这里是压缩空间之后的写法。

（因为感觉有些场景下python性能不太够，所以最近开始学习go语言，后面更新的博客内容我会尽量使用两种语言同步更新。后面也可能会专门写几期go语言的学习记录。）

### 复杂度分析

只从前向后推导一遍，时间复杂度为O(N)。

不压缩空间的话空间复杂度为O(N)，压缩空间的，空间复杂度为O(1)。

