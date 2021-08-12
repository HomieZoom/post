---
title:【leetcod每日一题】2021-8-12/[516. 最长回文子序列]
date:2021-8-12
tags:Leetcode
authoe:mili





---

## 【leetcod每日一题】2021-8-12[[516. 最长回文子序列](https://leetcode-cn.com/problems/longest-palindromic-subsequence/)]

### 题目

给你一个字符串 s ，找出其中最长的回文子序列，并返回该序列的长度。

子序列定义为：不改变剩余字符顺序的情况下，删除某些字符或者不删除任何字符形成的一个序列。



示例 1：

输入：s = "bbbab"
输出：4
解释：一个可能的最长回文子序列为 "bbbb" 。
示例 2：

输入：s = "cbbd"
输出：2
解释：一个可能的最长回文子序列为 "bb" 。


提示：

1 <= s.length <= 1000
s 仅由小写英文字母组成



### 题解

本题可以理解为[[5. 最长回文子串](https://leetcode-cn.com/problems/longest-palindromic-substring/)]的加强版，这题的有一个比较有意思的解可以好好看看。

回到本题，看到本题，因为要求子序列，所以其实一开始是有想到DP的，但是状态转移方程一直没想出来。所以最后还是没骨气的抄了答案，下面的解析其实也是力扣的官方解答。

所以其实本题的关键是DP的状态状态转移需要从后往前转移。使用 dp [i] [j]表示i 到 j 区间的最长回文子序列。所以若 nums[i] == nums[j] dp[i] [j] = dp [i+1] [j -1] + 2。否则 dp [i] [j] = max(dp [i + 1] [j], dp [i] [j -1])。

所以，代码就很容易写出来了：

```python
class Solution:
    def longestPalindromeSubseq(self, s: str) -> int:
        length = len(s)
        dp = [[0]*length for _ in range(length)]
        for i in range(length - 1, -1, -1):
            dp[i][i] = 1
            for j in range(i + 1, length):
                if s[i] == s[j]:
                    dp[i][j] = dp[i + 1][j - 1] + 2
                else:
                    dp[i][j] = max(dp[i + 1][j], dp[i][j - 1])

        return dp[0][length - 1]
```

go 代码如下：

```go
func longestPalindromeSubseq(s string) int {
    length := len(s)
    dp := make([][]int, length)
    for i := 0;i < length;i++ {
        dp[i] = make([]int, length)
    }
     for i := length - 1; i >= 0; i-- {
         dp[i][i] = 1
         for j := i + 1; j < length; j++ {
             if s[i] == s[j] {
                 dp[i][j] = dp[i + 1][j - 1] + 2
             } else {
                 dp[i][j] = max(dp[i + 1][j], dp[i][j - 1])
             }
         }
     }

    return dp[0][length - 1]
}

func max(a,b int) int {
    if a > b {
        return a
    }
    return b
} 
```



### 复杂度分析

时间复杂度O(n^2)，空间复杂度O(n^2)。