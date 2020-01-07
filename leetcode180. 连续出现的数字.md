# leetcode[180. 连续出现的数字](https://leetcode-cn.com/problems/consecutive-numbers/)

`类型：数据库`

## 1.问题描述

```
编写一个 SQL 查询，查找所有至少连续出现三次的数字。

+----+-----+
| Id | Num |
+----+-----+
| 1  |  1  |
| 2  |  1  |
| 3  |  1  |
| 4  |  2  |
| 5  |  1  |
| 6  |  2  |
| 7  |  2  |
+----+-----+
例如，给定上面的 Logs 表， 1 是唯一连续出现至少三次的数字。

+-----------------+
| ConsecutiveNums |
+-----------------+
| 1               |
+-----------------+

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/consecutive-numbers
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```

## 2.解题思路

### **【题目】**

下面是学生的成绩表（表名score，列名：学号、成绩），使用SQL查找所有至少连续出现3次的成绩。

![](https://pic.leetcode-cn.com/9a0dc9f43fb25c86cabd1f82e707b4501614451b4380ce429770b1bb14658ec4-1.png)

例如，“成绩”这一列里84是连续出现3次的成绩。

### 【解题思路】

1.什么是连续出现3次？
假设“学号”是按顺序排列的（如果不是，可以使用增加一列，让学号是按序号顺序排列的），所以每一学号与上一学号相差1。例如下图的3个学号是连续学号，他们之间的关系是：

某一学号（0002）=下一位的学号（0003）-1
下一位学号（0003）=下下位学号（0004）-1

![](https://pic.leetcode-cn.com/07dd2a1931b56dd2628cb0a55fef788045d1b882c9160bfd40b9be27a366b54c-a1.jpg)

2.如果这3个连续学号的成绩相等，就是题目要求的“至少连续出现3次的成绩”。

3.利用“自连接（自身连接）“的思路

自连接（自身连接）的本质是把一张表复制出多张一模一样的表来使用。SQL语法：

select 列明 
from 表名 as 别名1,表名 as 别名2;
步骤1）将成绩表（score）复制3分，分别命名为a、b、c

select *
from score as a,
   score as b,
   score as c;

步骤2）我们需要找到这3个表中3个连续的学号，这个条件如下
a.学号 = b.学号-1 and b.学号 = c.学号-1

![](https://pic.leetcode-cn.com/5c706c170ec6a14a60f5a9da81cc0bc16ad8a99a5cb2d566b8499c5ae0ae069b-a2.jpg)

步骤3）还要让这3个学号连续的人“成绩相等”，这个条件如下
a.成绩 = b.成绩 and b.成绩 = c.成绩

将步骤2和步骤3的条件合并起来就是下面SQL里的where字句：

select *
from score as a,
   score as b,
   score as c;
 where a.学号 = b.学号 - 1
   and b.学号 = c.学号 - 1
   and a.成绩 = b.成绩
   and b.成绩 = c.成绩;
步骤4）前面步骤已经将连续3人相等的成绩找出，现在用distinct去掉自连接产生的重复数。最终SQL如下：

select distinct a.成绩 as 最终答案
from score as a,
   score as b,
   score as c;
 where a.学号 = b.学号 - 1
   and b.学号 = c.学号 - 1
   and a.成绩 = b.成绩
   and b.成绩 = c.成绩;

### 本题答案

```
select distinct a.Num as ConsecutiveNums
    from Logs a,Logs b,Logs c 
    where 
        a.Id = b.Id-1 and
        b.Id = c.Id-1 and
        a.Num = b.Num and 
        b.Num = c.Num;
```

## [可以使用表连接来加快速度](https://leetcode-cn.com/problems/consecutive-numbers/solution/ke-yi-shi-yong-biao-lian-jie-lai-jia-kuai-su-du-by/)

```
select 
    distinct l1.Num AS ConsecutiveNums 
from 
    Logs l1 left join Logs l2 on l1.Num = l2.Num
    left join Logs l3 on l2.Num = l3.Num
where 
    l1.id = l2.id -1 and l2.id = l3.id -1
```

![](https://pic.leetcode-cn.com/16a4a39894a6563b805d66307a6fb1f6de21e39ddf203a4d11607bc35dd00156-image.png)



## 3【本题考点】

• 本题考察的是连续出现，会有同学忽略“连续”二字
• 考察对自关联的灵活应用
• 从题目连续3次成绩相等，判断出“成绩相等”和“学号连续”这2个条件。考察构建“连续学号成绩相等”的思维构建能力

### 【举一反三】

遇到类似“连续出N次的问题”可以回想本题的解答思路，如：查询至少连续3天没有出勤的员工。
