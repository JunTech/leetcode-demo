# leetcode181-[ 超过经理收入的员工](https://leetcode-cn.com/problems/employees-earning-more-than-their-managers/)

`类型:数据库`

## 1.问题描述

```
Employee 表包含所有员工，他们的经理也属于员工。每个员工都有一个 Id，此外还有一列对应员工的经理的 Id。

+----+-------+--------+-----------+
| Id | Name  | Salary | ManagerId |
+----+-------+--------+-----------+
| 1  | Joe   | 70000  | 3         |
| 2  | Henry | 80000  | 4         |
| 3  | Sam   | 60000  | NULL      |
| 4  | Max   | 90000  | NULL      |
+----+-------+--------+-----------+
给定 Employee 表，编写一个 SQL 查询，该查询可以获取收入超过他们经理的员工的姓名。在上面的表格中，Joe 是唯一一个收入超过他的经理的员工。

+----------+
| Employee |
+----------+
| Joe      |
+----------+

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/employees-earning-more-than-their-managers
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```

## 2.解题思路

### 2.1 使用where

```
从两个表里使用 Select 语句可能会导致产生 笛卡尔乘积 。在这种情况下，输出会产生 4*4=16 个记录。然而我们只对雇员工资高于经理的人感兴趣。所以我们应该用 WHERE 语句加 2 个判断条件。

代码：
SELECT
    a.name as Employee
FROM
    Employee AS a,
    Employee AS b
WHERE
    a.ManagerId = b.Id
        AND a.Salary > b.Salary
;
运行结果：
执行用时 :
269 ms
, 在所有 mysql 提交中击败了
31.62%
的用户
内存消耗 :
0B
, 在所有 mysql 提交中击败了
100.00%
的用户
```

### 2.2 使用inner join

```
代码：
	SELECT a.name as Employee from Employee a inner join Employee b on a.ManagerId = b.id and a.Salary > b.Salary;
执行结果：
3 分钟前	通过	542 ms	0B	Mysql
```

