---
title:【leetcod每日一题】2021-8-11/[446. 等差数列划分 II - 子序列]
date:2021-8-11
tags:Leetcode
authoe:mili




---

## 【leetcod每日一题】2021-8-11[[446. 等差数列划分 II - 子序列](https://leetcode-cn.com/problems/arithmetic-slices-ii-subsequence/)]

### 题目

给你一个整数数组 nums ，返回 nums 中所有 等差子序列 的数目。

如果一个序列中 至少有三个元素 ，并且任意两个相邻元素之差相同，则称该序列为等差序列。

例如，[1, 3, 5, 7, 9]、[7, 7, 7, 7] 和 [3, -1, -5, -9] 都是等差序列。
再例如，[1, 1, 2, 5, 7] 不是等差序列。
数组中的子序列是从数组中删除一些元素（也可能不删除）得到的一个序列。

例如，[2,5,10] 是 [1,2,1,2,4,1,5,10] 的一个子序列。
题目数据保证答案是一个 32-bit 整数。

 

示例 1：

输入：nums = [2,4,6,8,10]
输出：7
解释：所有的等差子序列为：
[2,4,6]
[4,6,8]
[6,8,10]
[2,4,6,8]
[4,6,8,10]
[2,4,6,8,10]
[2,6,10]
示例 2：

输入：nums = [7,7,7,7,7]
输出：16
解释：数组中的任意子序列都是等差子序列。


提示：

1  <= nums.length <= 1000
-231 <= nums[i] <= 231 - 1

### 题解

本题跟昨天的题一脉相承。但是增加了难度，之前的题目是计算等差的子数组，所以可以通过遍历一遍数组求得解，但是现在需要计算子序列，就无法通过一遍遍历求解了。所以必须通过双层遍历求得目标解。因为子序列不是连续的，为了获取全部的可能性，需要计算所有可能的等差数列公差。所以在外层for i in range（len（nums）遍历，在内层 for j in range(i)遍历。针对所以公差 d = nums[i] - nums[j] 寻找是否构成等差数列。如果每次都向前延伸就时间复杂度太高了，所以还是使用动态规划的思路，将之前的结果哈希记录，后面只需要查表就好了。代码如下：

```python
class Solution:
    def numberOfArithmeticSlices(self, nums: List[int]) -> int:
        ans = 0 
        temp = [defaultdict(int) for _ in nums]
        for index, num in enumerate(nums):
            for j in range(index):
                d = num - nums[j]
                part = temp[j][d]
                ans += part 
                temp[index][d] += part + 1
                
        return ans 
```



其中temp为一个哈希表列表，记录的是对应元素以公差d目前已经形成的等差数列长度，2,4记为1个等差长度，所以当下次遇到公差为2时，则ans += 1，并且将1+1计入下一公差为2的位置。（为什么是increas参考上一题。）。temp为什么是+=呢？其实是因为nums数组存在相等元素的情况。所以相同元素相同公差也可能会重复计算。

这个题解描述其实写的挺乱的，但是仔细看看代码，思索一下，其实还是可以想明白的。

```go
func numberOfArithmeticSlices(nums []int) int {
    ans := 0
    temp := make([]map[int]int, len(nums))
    for index, num := range nums {
        temp[index] = map[int]int{}
        for j, y := range nums[:index] {
            d := num - y
            part := temp[j][d]
            ans += part 
            temp[index][d] += part + 1  
        }
    }
    
    return ans 
}
```



### 复杂度分析

时间复杂度O(N^2)，空间复杂度O(N^2)