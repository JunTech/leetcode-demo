# leetcode183-[从不订购的客户](https://leetcode-cn.com/problems/customers-who-never-order/)

`类型：数据库`

## 1.问题描述

```
某网站包含两个表，Customers 表和 Orders 表。编写一个 SQL 查询，找出所有从不订购任何东西的客户。

Customers 表：

+----+-------+
| Id | Name  |
+----+-------+
| 1  | Joe   |
| 2  | Henry |
| 3  | Sam   |
| 4  | Max   |
+----+-------+
Orders 表：

+----+------------+
| Id | CustomerId |
+----+------------+
| 1  | 3          |
| 2  | 1          |
+----+------------+
例如给定上述表格，你的查询应返回：

+-----------+
| Customers |
+-----------+
| Henry     |
| Max       |
+-----------+

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/customers-who-never-order
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```

## 2.解题思路

### 2.1使用子查询和 `NOT IN` 子句

```
代码：
select customers.name as 'Customers'
from customers
where customers.id not in
(
    select customerid from orders
);

执行时间：
执行用时 :
194 ms
, 在所有 mysql 提交中击败了
22.41%
的用户
内存消耗 :
0B
, 在所有 mysql 提交中击败了
100.00%
的用户
```

