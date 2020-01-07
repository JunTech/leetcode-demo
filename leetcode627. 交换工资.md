# leetcode[627. 交换工资](https://leetcode-cn.com/problems/swap-salary/)

`类型：数据库`

## 1.问题描述

```
给定一个 salary 表，如下所示，有 m = 男性 和 f = 女性 的值。交换所有的 f 和 m 值（例如，将所有 f 值更改为 m，反之亦然）。要求只使用一个更新（Update）语句，并且没有中间的临时表。

注意，您必只能写一个 Update 语句，请不要编写任何 Select 语句。

例如：

| id | name | sex | salary |
|----|------|-----|--------|
| 1  | A    | m   | 2500   |
| 2  | B    | f   | 1500   |
| 3  | C    | m   | 5500   |
| 4  | D    | f   | 500    |
运行你所编写的更新语句之后，将会得到以下表:

| id | name | sex | salary |
|----|------|-----|--------|
| 1  | A    | f   | 2500   |
| 2  | B    | m   | 1500   |
| 3  | C    | f   | 5500   |
| 4  | D    | m   | 500    |

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/swap-salary
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```

## 2.解题思路

### 2.1CASE-WHEN

我们在这里使用case...when..来解决问题

![](https://pic.leetcode-cn.com/3eaf0e8682f22cc16f94fe93ed6ef8bec332af9b38bda1428bbbffa656b425c1-%E5%B9%BB%E7%81%AF%E7%89%8754.JPG)

更新语句时需要用到update语句，update语句使用方法如下：

```
update 表名 set 列名 = 修改后的值
```

本题答案：

```
update salary
set sex = (case sex
               when 'm' then 'f'
               else 'm'
           end);

```

### 2.2【举一反三】

在遇到需要将表内某列特定值替换成其他值时，记住case表达式如何使用。

本题如果只是要求查询的话，使用select语句即可：
select (上面的case表达式) from 表名

需要直接更新表中的数据的情况，熟记update语句。但要注意，在使用update更新表数据前，最好先将原表备份。

“按条件修改表数据”的应用例题

如下图所示的salary表，有m = 男性和f = 女性的值。交换所有的f和m的值（将所有f更改为m，m改为f）。要求只使用update的语句。

