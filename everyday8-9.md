---
title:【leetcod每日一题】2021-8-7/[313. 超级丑数]
date:2021-8-7
tags:Leetcode
authoe:mili




---

## 【leetcod每日一题】2021-8-7[[313. 超级丑数](https://leetcode-cn.com/problems/super-ugly-number/)]

### 题目

超级丑数 是一个正整数，并满足其所有质因数都出现在质数数组 primes 中。

给你一个整数 n 和一个整数数组 primes ，返回第 n 个 超级丑数 。

题目数据保证第 n 个 超级丑数 在 32-bit 带符号整数范围内。

 

示例 1：

输入：n = 12, primes = [2,7,13,19]
输出：32 
解释：给定长度为 4 的质数数组 primes = [2,7,13,19]，前 12 个超级丑数序列为：[1,2,4,7,8,13,14,16,19,26,28,32] 。
示例 2：

输入：n = 1, primes = [2,3,5]
输出：1
解释：1 不含质因数，因此它的所有质因数都在质数数组 primes = [2,3,5] 中。

提示：

1 <= n <= 106
1 <= primes.length <= 100
2 <= primes[i] <= 1000
题目数据 保证 primes[i] 是一个质数
primes 中的所有值都 互不相同 ，且按 递增顺序 排列

### 题解

不得不吐槽一下这道题，看题目就看了半天，有点像是阅读理解题，可能是因为题目是机翻。总结一下其实就是使用给定的质数，求第n个目标数，目标数的全部质因数均在给定的列表中。

其实看到这种求第N个得题目就知道哟啊使用动态规划。只是一开始没想到该怎么规划罢了。

本题中需要申请一个额外的数组，长度为len(primes)。用来记录当前的prime数组中对应的元素应该与那个已有的dp中的结果相乘。该辅助数组初始全部为1。

状态转移方程则为：dp[i] = min(dp[point[j]] * primes[j] for j in range(len(primes)))

完整的代码如下：

```python
class Solution:
    def nthSuperUglyNumber(self, n: int, primes: List[int]) -> int:
        dp = [0] * (n + 1)
        dp[1] = 1
        length = len(primes)
        pointer = [1] * length
        for i in range(2, n):
            min_mum = min(dp[pointer[j]] * primes[j] for j in range(length))
            dp[i] = min_num
            for j in range(length):
                if dp[pointer[j]] * primes[j] == min_num:
                    pointer[j] += 1
                    
        return dp[n]
```

另外附上go代码：

```go
func nthSuperUglyNumber(n int, primes []int) int {
    dp := make([]int, n + 1)
    dp[1] = 1
    m := len(primes)
    pointer := make([]int, m)
    for i := 0; i < m; i++ {
        pointer[i] = 1
    }

    for i := 2; i< n + 1; i++ {
        nums := make([]int, m)
        minNum := math.MaxInt64
        for j, p := range pointer {
            nums[j] = dp[p] * primes[j]
            minNum = min(minNum, nums[j])
        }
        dp[i] = minNum
        for j, num := range nums {
            if minNum == num {
                pointer[j] += 1
            }
        }
    }
    return dp[n]
}

func min(a, b int) int {
    if a < b {
        return a
    }
    return b
}
```



### 复杂度分析

时间复杂度O(N)，空间复杂度O(N)