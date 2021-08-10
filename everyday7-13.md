---
title:【leetcod每日一题】2021-7-13/[218. 天际线问题]
date:2021-7-13
tags:Leetcode
authoe:mili
---

## 【leetcod每日一题】2021-7-13/[[218. 天际线问题](https://leetcode-cn.com/problems/the-skyline-problem/)]

### 题目

城市的天际线是从远处观看该城市中所有建筑物形成的轮廓的外部轮廓。给你所有建筑物的位置和高度，请返回由这些建筑物形成的 天际线 。

每个建筑物的几何信息由数组 buildings 表示，其中三元组 buildings[i] = [lefti, righti, heighti] 表示：

lefti 是第 i 座建筑物左边缘的 x 坐标。
righti 是第 i 座建筑物右边缘的 x 坐标。
heighti 是第 i 座建筑物的高度。
天际线 应该表示为由 “关键点” 组成的列表，格式 [[x1,y1],[x2,y2],...] ，并按 x 坐标 进行 排序 。关键点是水平线段的左端点。列表中最后一个点是最右侧建筑物的终点，y 坐标始终为 0 ，仅用于标记天际线的终点。此外，任何两个相邻建筑物之间的地面都应被视为天际线轮廓的一部分。

注意：输出天际线中不得有连续的相同高度的水平线。例如 [...[2 3], [4 5], [7 5], [11 5], [12 7]...] 是不正确的答案；三条高度为 5 的线应该在最终输出中合并为一个：[...[2 3], [4 5], [12 7], ...]

 

示例 1：

输入：buildings = [[2,9,10],[3,7,15],[5,12,12],[15,20,10],[19,24,8]]
输出：[[2,10],[3,15],[7,12],[12,0],[15,10],[20,8],[24,0]]
解释：
图 A 显示输入的所有建筑物的位置和高度，
图 B 显示由这些建筑物形成的天际线。图 B 中的红点表示输出列表中的关键点。
示例 2：

输入：buildings = [[0,2,3],[2,5,3]]
输出：[[0,3],[5,0]]


提示：

1 <= buildings.length <= 104
0 <= lefti < righti <= 231 - 1
1 <= heighti <= 231 - 1
buildings 按 lefti 非递减排序

### 题解

题目文字描述有点单薄，md文件插图片又有点麻烦（主要是懒）。建议打开力扣题目链接配合食用。

本题标注为困难，实际坐下来，感觉也确实对得起困难这个难度。当然也可能是我菜。

看到本题说实话最开始是有点没思路的，然后想着遍历每个建筑，判断是否被当前建筑物遮挡，然后计算是否将对应的点加入到结果列表中。然后事实证明有点麻烦，我写不出来。

然后试着仔细分析了都是哪些点会被加入到结果列表里。根据题目描述，一个建筑物最多只会在列表中增加两个点，左上点和右下点。但是两个点能否加入到结果列表里都是有要求的，左上点如果在加入的时候是当前最高点，那么可以加入结果列表，右下点如果在建筑物生命周期结束的时候是最高点，那么右下点会加入到结果列表。这个时候我加入了一个生命周期的概念，其实是为了便于理解，因为我最开始也是从这个角度开始想的，顺着X轴逐步推进，然后依次处理每一个X点的高度。所以我这边在代码中将每一个建筑坐标拆成2个X坐标点的事件。例如[2,9,10]题中描述为x从2到9高度为10的建筑，在题解中拆为[1,10,True]和[9,10,False]两个元素，分别表示在x==2是high == 10开始生效，在x==9时，high==10生命周期结束。

所以此时解题思路就逐渐明晰了，先将各个建筑的左边按上述规则拆分为X点的事件,并存入temp数组，然后遍历该数组。若遇到新最大高度生效，则res列表中增加新点，否则不加。若愚最大高度失效，则res列表中增加新点，否则不加。所需要维护变量标记当前的最大高度，因为存在变量过期失效的情况，所以需要使用类似栈的数据结构来维护。

```python
class Solution:
    def getSkyline(self, buildings: List[List[int]]) -> List[List[int]]:
        res = []
        temp = []

        for item in buildings:
            start = item[0]
            end = item[1]
            high = item[2]
            temp.append([start,high,True])
            temp.append([end,high,False])

        high = [0]
        temp = sorted(temp, key = lambda x : x[0])
        for i in range(len(temp)):
            if temp[i][2]:
                if temp[i][1] > high[-1]:
                    high.append(temp[i][1])
                    high.sort()
                    if res and res[-1][0] == temp[i][0]:
                        res.pop()
                        if res and res[-1][1] == high[-1]:
                            continue

                    res.append([temp[i][0],high[-1]])
                else:
                    high.append(temp[i][1])
                    high.sort()

            else:
                if temp[i][1] == high[-1]:
                    high.remove(temp[i][1])
                    if temp[i][1] == high[-1]:
                        continue
                    if res and res[-1][0] == temp[i][0]:
                        res.pop()
                    res.append([temp[i][0],high[-1]])
                else:
                    high.remove(temp[i][1])
        return res
```

因为存在刚好在同一个X点旧的最大高度失效，同时增加新的最大高度，因为同一个X点只能有最多一个点，所以需要增加额外的判断，一般来说判断res[-1]的X是否和当前要加入的点的X是都相同，如果相同则pop掉res的最后元素即可，其中还有一个特殊情况就是，同一个最大高度，如果在该点失效，又重新生效，那么除了pop掉res尾元素之外，当前要添加的点也应该跳过。

除了上面说的因为加入新最大高度导致的res加入新点外，最大高度失效也会导致例外情况，如果最大高度失效，但是high栈中还有别的同样的高度在生效，例如当前最大高度为6，高度为6的建筑有3个，此时即使一个高度为6的建筑过期也res中也不会产生新的点。

处理完上面说的特殊情况然后就可以AC本题了。

（本题花费了大半天的时间，A出来还是蛮有成就感的。）

### 复杂度分析

遍历一遍原始数组，遍历一遍辅助数组（长度为2N），时间复杂度为O(3N)

额外申请了辅助数组的空间和高度列表的空间，空间复杂度为O(3N）