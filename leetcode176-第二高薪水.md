# leetcode176-第二高薪水

`类型：数据库`

## 1.问题描述

```
编写一个 SQL 查询，获取 Employee 表中第二高的薪水（Salary） 。

+----+--------+
| Id | Salary |
+----+--------+
| 1  | 100    |
| 2  | 200    |
| 3  | 300    |
+----+--------+
例如上述 Employee 表，SQL查询应该返回 200 作为第二高的薪水。如果不存在第二高的薪水，那么查询应返回 null。

+---------------------+
| SecondHighestSalary |
+---------------------+
| 200                 |
+---------------------+

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/second-highest-salary
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```

## 2.解题思路

1.判断是否有第二高，没有就是null，有就返回那个第二高薪水,这里使用sql中的IFNULL判断

2.查找第二高薪水：降序，limit1,1 

3.查出来使用别名赋值

## 3.代码

```sql
# Write your MySQL query statement below
select 
	IFNULL((select distinct(salary) 
from Employee 
	order by salary 
	desc limit 1,1),null) as  SecondHighestSalary;
```

执行结果：

通过

执行用时 :105 ms, 在所有 mysql 提交中击败了97.42%的用户

内存消耗 :0B, 在所有 mysql 提交中击败了100.00%的用户

| 提交时间 | 提交结果 | 执行用时 | 内存消耗 | 语言  |
| -------- | -------- | -------- | -------- | ----- |
| 几秒前   | [通过]() | 105 ms   | 0B       | Mysql |