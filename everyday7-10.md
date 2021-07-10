---
title:【leetcod每日一题】2021-7-10/[981. 基于时间的键值存储]
date:2021-7-10
tags:Leetcode
authoe:mili
---

# 【leetcod每日一题】2021-7-10/[981. 基于时间的键值存储]

## 题目

创建一个基于时间的键值存储类 TimeMap，它支持下面两个操作：

1. set(string key, string value, int timestamp)

存储键 key、值 value，以及给定的时间戳 timestamp。
2. get(string key, int timestamp)

返回先前调用 set(key, value, timestamp_prev) 所存储的值，其中 timestamp_prev <= timestamp。
如果有多个这样的值，则返回对应最大的  timestamp_prev 的那个值。
如果没有值，则返回空字符串（""）。


示例 1：

输入：inputs = ["TimeMap","set","get","get","set","get","get"], inputs = [[],["foo","bar",1],["foo",1],["foo",3],["foo","bar2",4],["foo",4],["foo",5]]
输出：[null,null,"bar","bar",null,"bar2","bar2"]
解释：  
TimeMap kv;   
kv.set("foo", "bar", 1); // 存储键 "foo" 和值 "bar" 以及时间戳 timestamp = 1   
kv.get("foo", 1);  // 输出 "bar"   
kv.get("foo", 3); // 输出 "bar" 因为在时间戳 3 和时间戳 2 处没有对应 "foo" 的值，所以唯一的值位于时间戳 1 处（即 "bar"）   
kv.set("foo", "bar2", 4);   
kv.get("foo", 4); // 输出 "bar2"   
kv.get("foo", 5); // 输出 "bar2"   

示例 2：

输入：inputs = ["TimeMap","set","set","get","get","get","get","get"], inputs = [[],["love","high",10],["love","low",20],["love",5],["love",10],["love",15],["love",20],["love",25]]
输出：[null,null,null,"","high","high","low","low"]

## 题解

题目有点长，看着有点烦。懒得再赘述了，因为也没什么人看，其实也就是自己记录一下罢了。

这种类设计的题目其实做的不太多，但是其实看下来这道题难度不算太大。如果不考虑时间要求的话，基本上是有手就行的程度了。也就勉强对得起一道中等题的身份吧。话不多说先上代码吧

```python
from collections import defaultdict
class TimeMap:

    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.status = defaultdict(list)

    def set(self, key: str, value: str, timestamp: int) -> None:
        self.status[key].append((value, timestamp))
        


    def get(self, key: str, timestamp: int) -> str:
        self.status[key] = sorted(self.status[key], key = lambda x: x[1],reverse = True)
        for res_key, res_time in self.status[key]:
            if res_time <= timestamp:
                return res_key
            else:
                continue

        else:
            return ""



# Your TimeMap object will be instantiated and called as such:
# obj = TimeMap()
# obj.set(key,value,timestamp)
# param_2 = obj.get(key,timestamp)
```

这样写其实讲道理是跑得通的，但是会在大样本的用例处失败。

其实个人感觉这道理可以称得上是难点的地方也就1个吧，就是选用什么样的数据结构来存储各种数据，兼顾空间复杂度和get操作的时间复杂度。

上面的这段代码会失败，其实很简单，因为相当于是记录了所以的信息，对一些无用信息并没有过滤。而且题目里面表明了时间值在存入的时候是严格递增的，但是我还是倔强的进行了一次排序。

简单修改之后又如下代码：

```python
from collections import defaultdict
class TimeMap:

    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.status = defaultdict(list)

    def set(self, key: str, value: str, timestamp: int) -> None:
        self.status[key].append((value, timestamp))
        


    def get(self, key: str, timestamp: int) -> str:

        for res_key, res_time in self.status[key][::-1]:
            if res_time <= timestamp:
                return res_key
            else:
                continue

        else:
            return ""



# Your TimeMap object will be instantiated and called as such:
# obj = TimeMap()
# obj.set(key,value,timestamp)
# param_2 = obj.get(key,timestamp)
```

其实这里只是去掉了按个无用的排序，最后要去到返回值的时候还是要遍历比较多的样本。对输入的数据没有过滤。就是在set接口中，没有做任何操作就把数据丢进去，导致在取的时候有比较多的工作量。记录的时候其实是一条一条记录的，其实会有比较多的重复数据。因为在get操作的时候，其实是一段时间对应一个返回，所以其实应该在记录数据的时候就做好区分，在get的时候只要判断时间点是否在对应的区间，然后返回即可。

```python
from collections import defaultdict
class TimeMap:

    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.status = defaultdict(list)

    def set(self, key: str, value: str, timestamp: int) -> None:
        if not self.status[key] :
            self.status[key].append([value, timestamp, timestamp])
        elif value != self.status[key][-1][0]:
            self.status[key][-1][2] = timestamp
            self.status[key].append([value, timestamp, timestamp])
        else:
            self.status[key][-1][2] = timestamp


    def get(self, key: str, timestamp: int) -> str:
        if not self.status[key] or timestamp < self.status[key][0][1] :
            return ""
        for item in self.status[key]:
            if item[1] <= timestamp < item[2]:
                return item[0]
            else:
                continue

        else:
            return item[0]


# Your TimeMap object will be instantiated and called as such:
# obj = TimeMap()
# obj.set(key,value,timestamp)
# param_2 = obj.get(key,timestamp)
```

代码如上。

最终题解在python3提交中

执行用时：356 ms, 在所有 Python3 提交中击败了100.00%的用户

内存消耗：60 MB, 在所有 Python3 提交中击败了97.96%的用户

（虽然没有什么卵用，但是并不妨碍我开心一下）

## 复杂度分析

这题我一下子不知道该怎么去分析复杂度了，就偷个懒不分析了。