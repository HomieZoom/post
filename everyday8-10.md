---
title:【leetcod每日一题】2021-8-10/[413. 等差数列划分]
date:2021-8-10
tags:Leetcode
authoe:mili



---

## 【leetcod每日一题】2021-8-10[[413. 等差数列划分](https://leetcode-cn.com/problems/arithmetic-slices/)]

### 题目

如果一个数列 至少有三个元素 ，并且任意两个相邻元素之差相同，则称该数列为等差数列。

例如，[1,3,5,7,9]、[7,7,7,7] 和 [3,-1,-5,-9] 都是等差数列。
给你一个整数数组 nums ，返回数组 nums 中所有为等差数组的 子数组 个数。

子数组 是数组中的一个连续序列。

 

示例 1：

输入：nums = [1,2,3,4]
输出：3
解释：nums 中有三个子等差数组：[1, 2, 3]、[2, 3, 4] 和 [1,2,3,4] 自身。
示例 2：

输入：nums = [1]
输出：0

### 题解

本题可用动态规划求解。连续3个数字组成等差数列等差则结果+1。连续4个数字足称等差数列则相当于有2个长度为3的等差数列和一个长度为4的等差数列共计3个等差数列。类推长度为5的等差数列共计有6。可以找到规律，随等差数列长度的递增，子数组的增量每次+1。可以得到如下代码：

```python
class Solution:
    def numberOfArithmeticSlices(self, A: List[int]) -> int:
        length = len(A)
        if length < 3:
            return 0
        ans = 0
        increas = 0

        for i in range(2, length):
            if A[i] - A[i - 1] == A[i - 1] - A[i - 2] :
                increas += 1

            else:

                increas = 0

            ans += increas

        return ans
```

上面是空间压缩过的动态规划模式。如果是连续的等差数列，则ans += increas,其中increase随当前等差长度递增。当当前的等差数列中断时increas归零，等待下次形成等差数列再递增。

```go
func numberOfArithmeticSlices(nums []int) int {
    length := len(nums)
    if length < 3 {
        return 0
    }

    ans := 0
    increas := 0

    for i := 2; i < length; i++ {
        if nums[i] - nums[i - 1] == nums[i -1] - nums[i - 2] {
            increas += 1
        } else {
            increas = 0
        }
        ans += increas
    }
    return ans
}
```

go代码也是相同的逻辑。

### 复杂度分析

遍历一遍数组，时间复杂度为O(N)。只申请了固定的变量，空间复杂度为O(1)。