# leetcode175-组合两个表

`类型：数据库`

## 1.问题描述

```
表1: Person

+-------------+---------+
| 列名         | 类型     |
+-------------+---------+
| PersonId    | int     |
| FirstName   | varchar |
| LastName    | varchar |
+-------------+---------+
PersonId 是上表主键
表2: Address

+-------------+---------+
| 列名         | 类型    |
+-------------+---------+
| AddressId   | int     |
| PersonId    | int     |
| City        | varchar |
| State       | varchar |
+-------------+---------+
AddressId 是上表主键
 

编写一个 SQL 查询，满足条件：无论 person 是否有地址信息，都需要基于上述两表提供 person 的以下信息：

 

FirstName, LastName, City, State

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/combine-two-tables
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```

## 2.解题思路

因为表 Address 中的 personId 是表 Person 的外关键字，所以我们可以连接这两个表来获取一个人的地址信息。

考虑到可能不是每个人都有地址信息，我们应该使用 outer join 而不是默认的 inner join

```
select FirstName, LastName, City, State
from Person left join Address
on Person.PersonId = Address.PersonId
;
```

## 3.运行结果

输入

{"headers": {"Person": ["PersonId", "LastName", "FirstName"], "Address": ["AddressId", "PersonId", "City", "State"]}, "rows": {"Person": [[1, "Wang", "Allen"]], "Address": [[1, 2, "New York City", "New York"]]}}

输出

{"headers": ["FirstName", "LastName", "City", "State"], "values": [["Allen", "Wang", null, null]]}

差别

预期结果

{"headers": ["FirstName", "LastName", "City", "State"], "values": [["Allen", "Wang", null, null]]}



```
执行结果：
通过
显示详情
执行用时 :
152 ms
, 在所有 mysql 提交中击败了
99.85%
的用户
内存消耗 :
0B
, 在所有 mysql 提交中击败了
100.00%
的用户
```

| 提交时间 | 提交结果 | 执行用时 | 内存消耗 | 语言  |
| -------- | -------- | -------- | -------- | ----- |
| 几秒前   | [通过]() | 152 ms   | 0B       | Mysql |