---
title:【leetcod每日一题】2021-7-8/[930. 和相同的二元子数组]
date:2021-7-8
tags:Leetcode
authoe:mili
---

## 【leetcod每日一题】2021-7-8/[930. 和相同的二元子数组]



### 题目

给你一个二元数组 nums ，和一个整数 goal ，请你统计并返回有多少个和为 goal 的 非空 子数组。

子数组 是数组的一段连续部分。

 

示例 1：

输入：nums = [1,0,1,0,1], goal = 2
输出：4
解释：
有 4 个满足题目要求的子数组：[1,0,1]、[1,0,1,0]、[0,1,0,1]、[1,0,1]
示例 2：

输入：nums = [0,0,0,0,0], goal = 0
输出：15


提示：

1 <= nums.length <= 3 * 104
nums[i] 不是 0 就是 1
0 <= goal <= nums.length

### 题解

看题目第一眼其实就是一道双指针的题，类似于twosum，可以理解为是一道n sum的题。要求返回的是子数组，所以注定是不能使用排序数组解了。但是题目有限制表中元素只可能是0和1。所以子数组的和是严格随着子数组的规模递增的。

看到这题最开始的想法是暴力解，只要循环套循环不就好了么？所以有了如下代码。

```python
class Solution:
    def numSubarraysWithSum(self, nums: List[int], goal: int) -> int:
        length =  len(nums)
        l , r = 0, 0
        res = 0
        cur = 0
        while l < length:
            r = l
            cur = 0
            while r < length:

                if cur + nums[r] == goal:
                    res += 1
                    cur += nums[r]
                    r += 1
                elif cur + nums[r] < goal:

                    cur += nums[r]
                    r += 1
                elif cur + nums[r] > goal:
                    break
                    
            l += 1

        return res
```

这个题解，倒也不能说是错，只不过很朴素就是了。朴素到leetcode提交超时，，，

这样解，时间复杂度是O(n^2)级别，空间复杂度O(1)。

既然超时了那就只能想办法空间换时间了。从给定数组中求子数组，使子数组之和等于goal。可以使用前缀数组加双指针的方法。presum[r] - presum[l] == goal,res  += 1即可。但是仔细一想，这样是不是也得使用双层循环嵌套么，时间复杂度相当于还是O(n ^ 2)。所以再引入哈希表，记录当前遍历元素左边的所有前缀数，这样就省去了内层的循环，只需要在哈希表中寻找期望的数值即可。于是有了以下代码：

```python
from collections import defaultdict
from itertools import accumulate
class Solution:
    def numSubarraysWithSum(self, nums, goal):
        length = len(nums)
        presum = [0] + accumulate(nums)
        hashmap = defaultdict(int, {0:1})
        res = 0
        for i in range(length):
            R = presum[i + 1]
            L = R - goal
            res += hashmap[L]
            hashmap[R] += 1
            
        return res 
```

首先import了 defaultdict和accumulate两个方法。这两个方法以及方法所在的模块个人感觉都经常会用到，还是有必要去好好看看的。

然后申请presum数组记录各元素的前缀数之和。若nums[L:R]题目所求的某一解，即 0到R的前缀和为presum[R]，0到L-1的前缀和为presume[L - 1], 则presume[R] -presum[L] == goal。 因为希望前缀和从0开始，所以再最前面加[0]作为辅助位，若无0会漏掉以数组第一位为L节点的子数组。所以再初始化哈希表的时候也要先初始化{0:1}。

### 复杂度分析

时间复杂度：单层循环时间复杂度为O(N).

空间复杂度：额外申请了哈希表的空间，空间复杂度为O(N)



