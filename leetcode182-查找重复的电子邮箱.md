# leetcode182-[查找重复的电子邮箱](https://leetcode-cn.com/problems/duplicate-emails/)

`类型：数据库`

## 1.题目描述

```
编写一个 SQL 查询，查找 Person 表中所有重复的电子邮箱。

示例：

+----+---------+
| Id | Email   |
+----+---------+
| 1  | a@b.com |
| 2  | c@d.com |
| 3  | a@b.com |
+----+---------+
根据以上输入，你的查询应返回以下结果：

+---------+
| Email   |
+---------+
| a@b.com |
+---------+
说明：所有电子邮箱都是小写字母。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/duplicate-emails
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```

## 2.解题思路

### 2.1使用inner join

```
代码：
select distinct p1.Email from Person p1 inner join Person p2 on p1.Id <> p2.Id and p1.Email = p2.Email;
执行：
执行用时 :
286 ms
, 在所有 mysql 提交中击败了
5.02%
的用户
内存消耗 :
0B
, 在所有 mysql 提交中击败了
100.00%
的用户
可以看到，效率极低
```

## 2.2groupby + 临时表

```
select Email from
(
  select Email, count(Email) as num
  from Person
  group by Email
) as statistic
where num > 1
;

```

## 2.3groupby+having

```
向 GROUP BY 添加条件的一种更常用的方法是使用 HAVING 子句，该子句更为简单高效。

所以我们可以将上面的解决方案重写为：

MySQL
select Email
from Person
group by Email
having count(Email) > 1;

```

