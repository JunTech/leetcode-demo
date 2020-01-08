# leetcode[184. 部门工资最高的员工](https://leetcode-cn.com/problems/department-highest-salary/)

`类型：数据库`

## 1.问题描述

```
Employee 表包含所有员工信息，每个员工有其对应的 Id, salary 和 department Id。

+----+-------+--------+--------------+
| Id | Name  | Salary | DepartmentId |
+----+-------+--------+--------------+
| 1  | Joe   | 70000  | 1            |
| 2  | Henry | 80000  | 2            |
| 3  | Sam   | 60000  | 2            |
| 4  | Max   | 90000  | 1            |
+----+-------+--------+--------------+
Department 表包含公司所有部门的信息。

+----+----------+
| Id | Name     |
+----+----------+
| 1  | IT       |
| 2  | Sales    |
+----+----------+
编写一个 SQL 查询，找出每个部门工资最高的员工。例如，根据上述给定的表格，Max 在 IT 部门有最高工资，Henry 在 Sales 部门有最高工资。

+------------+----------+--------+
| Department | Employee | Salary |
+------------+----------+--------+
| IT         | Max      | 90000  |
| Sales      | Henry    | 80000  |
+------------+----------+--------+

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/department-highest-salary
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```

## 2.解题思路

1.按照部门id查找出每个部门最大的工资

```
select max(e.Salary),e.DepartmentId from Employee e group by e.DepartmentId;
```

2.查找部门id和工资与1相同的员工

```
select d.NAME AS Department,
	e.NAME AS Employee,
	e.Salary 
from Employee e,Department d
where e.DepartmentId = d.Id and (Salary,DepartmentId) in (select max(e.Salary),e.DepartmentId from Employee e group by e.DepartmentId);
```



疑惑：在这里为什么不直接按照部门分组获取到最高工资的员工信息，因为group by 会默认返回第一个查询到的分组信息，所以在这里，你会发现使用group by 明明应该有3条结果，却只有2条而且还是最先出现部门不同的两条结果！