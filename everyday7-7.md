---
titile:【leetcod每日一题】2021-7-7/[1711. 大餐计数]
date:2021-7-8
tags:Leetcode
authoe:mili
---





# 【leetcod每日一题】2021-7-7/[1711. 大餐计数](https://leetcode-cn.com/problems/count-good-meals/)



## 题目:

大餐 是指 恰好包含两道不同餐品 的一餐，其美味程度之和等于 2 的幂。

你可以搭配 任意 两道餐品做一顿大餐。

给你一个整数数组 deliciousness ，其中 deliciousness[i] 是第 i 道餐品的美味程度，返回你可以用数组中的餐品做出的不同 大餐 的数量。结果需要对 109 + 7 取余。

注意，只要餐品下标不同，就可以认为是不同的餐品，即便它们的美味程度相同。

 

示例 1：

输入：deliciousness = [1,3,5,7,9]
输出：4
解释：大餐的美味程度组合为 (1,3) 、(1,7) 、(3,5) 和 (7,9) 。
它们各自的美味程度之和分别为 4 、8 、8 和 16 ，都是 2 的幂。
示例 2：

输入：deliciousness = [1,1,1,3,3,3,7]
输出：15
解释：大餐的美味程度组合为 3 种 (1,1) ，9 种 (1,3) ，和 3 种 (1,7)。

**提示：**

- `1 <= deliciousness.length <= 105`
- `0 <= deliciousness[i] <= 220`

## 题解

## 	

​	这道题在leetcode网站上标记为中等题。看到这道题其实就可以意识到是一道类似于两数之和的题目，只不过增加了判断是否为2的幂。对于判断2的幂，最朴素的方法可以单独写一个函数，连续除2并求余4判断。不过显然也可以使用别的更为巧妙的方法。

​	在本题解目中，因为`1 <= deliciousness.length <= 105` 所以，可以使用 1 << (0,22)来循环22次判断是否为2的幂次。相比之连续取余会更加高效美观一点。

废话不多说，先上代码：

```kotlin
(```)
from collections import defaultdict
class Solution:
    mod = 10 ** 9 + 7
    maxinum = 1 << 22 
    def countPairs(self, deliciousness: List[int]) -> int:
        hashmap = defaultdict(int)
        res = 0
        for cur in deliciousness:
            i = 1 
            while i < self.maxinum:
                temp = i - cur
                if temp in hashmap:
                    res += hashmap[temp]
                    if res >= self.mod:
                        res -= self.mod
                i <<= 1
            hashmap[cur] += 1
        return res
(```)
```

​    

首先在代码的最开始设置了mod和maxinum用来标记最大结果的模和过程中的最大数。算法开始前申请哈希表，初始表为空，用来记录元素出现个数。

迭代给定元素列表，对于每一个元素，依次用 2 的 0~22次方取差值，作为目标值，取哈希表中寻找目标数值。若哈希表中有该数字，则说明该数字temp和当前列表中数字cur之和为2的i次幂。所以结果res += hashmap[temp] 即可。

在i连续左移22位后达到题目设定的元素最大值，开始cur的下一次迭代。在cur下一次开始迭代之前将cur 放入哈希表中。



## 复杂度分析：

时间复杂度：哈希表存取的时间复杂度为O(1),最外层循环为O(n)，内层循环为O(21)，所以总的看下来时间复杂度为O(21n)，基本上还是在O(N)规模上的。

空间复杂度：额外申请的空间只有hashmap和其他变量，所以时间复杂度基本上是O(N)规模。