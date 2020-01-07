# leetcode[178. 分数排名](https://leetcode-cn.com/problems/rank-scores/)

`类型：数据库`

## 1.题目描述

```
SQL架构
编写一个 SQL 查询来实现分数排名。如果两个分数相同，则两个分数排名（Rank）相同。请注意，平分后的下一个名次应该是下一个连续的整数值。换句话说，名次之间不应该有“间隔”。

+----+-------+
| Id | Score |
+----+-------+
| 1  | 3.50  |
| 2  | 3.65  |
| 3  | 4.00  |
| 4  | 3.85  |
| 5  | 4.00  |
| 6  | 3.65  |
+----+-------+
例如，根据上述给定的 Scores 表，你的查询应该返回（按分数从高到低排列）：

+-------+------+
| Score | Rank |
+-------+------+
| 4.00  | 1    |
| 4.00  | 1    |
| 3.85  | 2    |
| 3.65  | 3    |
| 3.65  | 3    |
| 3.50  | 4    |
+-------+------+
```

## 2.解题思路

**【题目】**
下图是"班级"表中的内容，记录了每个学生所在班级，和对应的成绩。

![](https://pic.leetcode-cn.com/eea7ef5a2477a773d6ebbc04f53f701dbc59281983f8009b268ba6ae2cce04a1-1.png)

现在需要按成绩来排名，如果两个分数相同，那么排名要是并列的。

正常排名是1，2，3，4，但是现在前3名是并列的名次，排名结果是：1，1，1，2。

【解题思路】

1.涉及到排名问题，可以使用窗口函数

2.专用窗口函数rank, dense_rank, row_number有什么区别呢？

它们的区别我举个例子，你们一下就能看懂：

```
select *,
   rank() over (order by 成绩 desc) as ranking,
   dense_rank() over (order by 成绩 desc) as dese_rank,
   row_number() over (order by 成绩 desc) as row_num
from 班级
```

得到结果：

![](https://pic.leetcode-cn.com/555db2ac6d57cc9c591c6475de79262f7ba4ecd43142ff0750e09d4d18fdffa6-1.png)

从上面的结果可以看出：
1）rank函数：这个例子中是5位，5位，5位，8位，也就是如果有并列名次的行，会占用下一名次的位置。比如正常排名是1，2，3，4，但是现在前3名是并列的名次，结果是：1，1，1，4。

2）dense_rank函数：这个例子中是5位，5位，5位，6位，也就是如果有并列名次的行，不占用下一名次的位置。比如正常排名是1，2，3，4，但是现在前3名是并列的名次，结果是：1，1，1，2。

3）row_number函数：这个例子中是5位，6位，7位，8位，也就是不考虑并列名次的情况。比如前3名是并列的名次，排名是正常的1，2，3，4。

这三个函数的区别如下：![](https://pic.leetcode-cn.com/729cc8ee48f55e4c4c448d764e6c0c1e1de50a7cb1674fd557abff50519651a8-1.png)

根据题目要求的排名规则，这里我们使用dense_rank函数。所以，最终的sql语句是：

```
select *,

   dense_rank() over (order by 成绩 desc) as dese_rank

from 班级

```

## **3.【本题考点】**

1.考察如何使用窗口函数
2.专用窗口函数排名的区别：rank, dense_rank, row_number

涉及到排名的问题，都可以使用窗口函数来解决。记住rank, dense_rank, row_number排名的区别。

回到本题，参考答案：

```
select score, 
       dense_rank() over(order by Score desc) as Rank
from Scores;
```

## 4.解决问题

窗口函数解决的问题包括：1）排名问题2）top N问题