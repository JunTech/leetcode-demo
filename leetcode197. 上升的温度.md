# leetcode[197. 上升的温度](https://leetcode-cn.com/problems/rising-temperature/)

`类型：数据库`

## 1.问题描述

```
给定一个 Weather 表，编写一个 SQL 查询，来查找与之前（昨天的）日期相比温度更高的所有日期的 Id。

+---------+------------------+------------------+
| Id(INT) | RecordDate(DATE) | Temperature(INT) |
+---------+------------------+------------------+
|       1 |       2015-01-01 |               10 |
|       2 |       2015-01-02 |               25 |
|       3 |       2015-01-03 |               20 |
|       4 |       2015-01-04 |               30 |
+---------+------------------+------------------+
例如，根据上述给定的 Weather 表格，返回如下 Id:

+----+
| Id |
+----+
|  2 |
|  4 |
+----+

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/rising-temperature
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```

## 2.解题思路

在这里使用sql的日期比较函数DATEDIFF比较日期

首先认识一下 DATEDIFF 函数，可以计算两者的日期差

```
DATEDIFF('2007-12-31','2007-12-30');   # 1
DATEDIFF('2010-12-30','2010-12-31');   # -1
```

所以查询的条件有两个：

1. 与之前的日期相差为 1
2. 比之前的温度高

```
SELECT b.Id
FROM Weather as a,Weather as b
WHERE a.Temperature < b.Temperature and DATEDIFF(a.RecordDate,b.RecordDate) = -1;
```

