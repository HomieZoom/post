---
title:【leetcod每日一题】2021-7-9/[面试题 17.10. 主要元素]
date:2021-7-9
tags:Leetcode
authoe:mili
---

# 【leetcod每日一题】2021-7-9/[[面试题 17.10. 主要元素](https://leetcode-cn.com/problems/find-majority-element-lcci/)]

## 题目

数组中占比超过一半的元素称之为主要元素。给你一个 整数 数组，找出其中的主要元素。若没有，返回 -1 。请设计时间复杂度为 O(N) 、空间复杂度为 O(1) 的解决方案。

示例 1：

输入：[1,2,5,9,5,9,5,5,5]
输出：5
示例 2：

输入：[3,2]
输出：-1
示例 3：

输入：[2,2,1,1,1,2,2]
输出：2

## 题解

今日题目是一道简单题目，看完基本上就思路很清晰了。这里提供两种解法吧。

```python
import collections
class Solution:
    def majorityElement(self, nums: List[int]) -> int:
        n = len(nums)
        counter = collections.Counter(nums)
        counter = sorted(counter.items(),key = lambda x : x[1])
        if counter[-1] [1] > n // 2:
            return counter[-1][0]
        else:
            return -1
        
```

这里是直接只用了collections模块中的Counter方法，统计列表中个元素出现的频次，然后调用sorted方法，根据出现的次数排序，取出现次数最多的值。但是在函数返回的时候需要再判断一次该值出现次数是否超过列表的一般。这种解法相对比较朴素。

下面还有相对高级一点的解法（其实并没有）。使用摩尔计数法。至于为什么要叫摩尔计数其实我也不太清楚，反正大家都这么叫也就这么叫了。

题目要求找到列表中出现次数超过列表总数一半的元素，所以假定最元素为主要元素，并技术count = 1，然后遍历后续数组，遇到相同元素就count++ ，否则count--，一旦count == 0,则记新的元素为主要元素。其实思路就是不相同的元素互相抵消，最后剩下元素就有可能是主要元素。为什么要说有可能？因为如果像[1, 2, 3]这样的数组，肉眼可见是没有主要元素的，但是如果单纯按照上述方法遍历一遍数组，最后主要元素会记为3。所以在最后需要再验证一遍元素的出现次数是否超过一半。代码如下。

```python
class Solution:
    def majorityElement(self, nums: List[int]) -> int:
        major = nums[0]
        count = 1
        length = len(nums)
        for i in nums[1:]:
            if not count:
                major = i
                count = 1
            elif i == major:
                count += 1
            else:
                count -= 1

        return major if nums.count(major) > length // 2 else -1
```

其实本题还可以使用哈希表计数一旦某一元素出现次数超过 length // 2就返回该元素，否则返回-1。

（因为太菜了，所以只能在写一道简单题的多种解法。有点孔乙己茴香豆的四种写法的味道。）

## 复杂度分析

计数排序：

​	时间复杂度：计数接口的时间复杂度O(N),排序接口的时间复杂度O(N)。没有嵌套关系，所以总的来说时间复杂度还是O(N)级别的。

​	空间复杂度：额外申请了一个列表，空间复杂度O(N)

摩尔排序：

​	时间复杂度：遍历一遍数组，时间复杂度O(N)

​	空间复杂度：额外申请了3个变量，空间复杂度为O(1)

哈希计数：

​	和第一种解法相同。