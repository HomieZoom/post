---
title:【leetcod每日一题】2021-7-14/[1818. 绝对差值和]
date:2021-7-14
tags:Leetcode
authoe:mili
---

## 【leetcod每日一题】2021-7-14/[[1818. 绝对差值和](https://leetcode-cn.com/problems/minimum-absolute-sum-difference/)]

### 题目

给你两个正整数数组 nums1 和 nums2 ，数组的长度都是 n 。

数组 nums1 和 nums2 的 绝对差值和 定义为所有 |nums1[i] - nums2[i]|（0 <= i < n）的 总和（下标从 0 开始）。

你可以选用 nums1 中的 任意一个 元素来替换 nums1 中的 至多 一个元素，以 最小化 绝对差值和。

在替换数组 nums1 中最多一个元素 之后 ，返回最小绝对差值和。因为答案可能很大，所以需要对 109 + 7 取余 后返回。

|x| 定义为：

如果 x >= 0 ，值为 x ，或者
如果 x <= 0 ，值为 -x


示例 1：

输入：nums1 = [1,7,5], nums2 = [2,3,5]
输出：3
解释：有两种可能的最优方案：
- 将第二个元素替换为第一个元素：[1,7,5] => [1,1,5] ，或者
- 将第二个元素替换为第三个元素：[1,7,5] => [1,5,5]
两种方案的绝对差值和都是 |1-2| + (|1-3| 或者 |5-3|) + |5-5| = 3
示例 2：

输入：nums1 = [2,4,6,8,10], nums2 = [2,4,6,8,10]
输出：0
解释：nums1 和 nums2 相等，所以不用替换元素。绝对差值和为 0
示例 3：

输入：nums1 = [1,10,4,4,2,7], nums2 = [9,3,5,1,7,4]
输出：20
解释：将第一个元素替换为第二个元素：[1,10,4,4,2,7] => [10,10,4,4,2,7]
绝对差值和为 |10-9| + |10-3| + |4-5| + |4-1| + |2-7| + |7-4| = 20


提示：

n == nums1.length
n == nums2.length
1 <= n <= 105
1 <= nums1[i], nums2[i] <= 105

### 题解

难度为中等，本题没有昨天的题有难度，所以多多少少有点索然无味。

直接上代码吧

```python
class Solution:
    def minAbsoluteSumDiff(self, nums1: List[int], nums2: List[int]) -> int:
        K = pow(10,9)+7
        ans = 0
        reduce = 0
        l = set(nums1)
        for i in range(len(nums1)):
            a = abs(nums1[i]-nums2[i])
            ans += a
            ans = ans % K
            if a>reduce:
                for j in range(a-reduce):
                    if (nums2[i]+j) in l:
                        reduce = a-j
                        break
                    if (nums2[i]-j) in l:
                        reduce = a-j
                        break
        ans = ans - reduce
        ans = ans % K

        
        return ans
```

本题的思路其实就很简单，同时遍历nums1和nums2，计算对应数组的差的绝对值，边计算绝对差，边寻找nums[i]的替代品。可以将redunce理解为最后绝对差和的修正值，此值越大，ans越小。最初修正值为0，在遍历中寻找最大修正值。在这里使用set（nums1）主要是因为在set中找元素效率最高。

如果单个的差绝对值小于当前修正值，说明替换之后也不存在比当前修正值大的可能，然后就是沿着nums2[i]向两边依次外扩，判断l中是都有对应元素，若有则刷新修正值。

最后ans - 修正值，并取模即可。

### 复杂度分析

时间复杂度O(n + nlogn)。应该是这样吧？因为寻找修正时的循环应该是这个时间复杂度

空间复杂度O(nlong)