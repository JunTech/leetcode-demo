#  多数元素

## 1.问题描述

给定一个大小为 *n* 的数组，找到其中的多数元素。多数元素是指在数组中出现次数**大于** `⌊ n/2 ⌋` 的元素。

你可以假设数组是非空的，并且给定的数组总是存在多数元素。

**示例 1:**

```
输入: [3,2,3]
输出: 3
```

**示例 2:**

```
输入: [2,2,1,1,1,2,2]
输出: 2
```

## 2.解题思路

先定义一个半数(n/2),初始化一个计数的count为1，默认出现一次，最后把结果放入list中然后遍历比较其中的值，相等就计数器加1，判断计数器与半数的值大小，放入list中

```
class Solution {
    public int majorityElement(int[] nums) {
        int half_length = nums.length/2; //半数
        int count = 1; //记录该数出现的次数，默认出现一次
        List<Integer> list = new ArrayList<Integer>(1);
        for (int i = 0; i <nums.length ; i++) {
            for (int j = nums.length-1; j >i; j--) {
                if(nums[i]==nums[j]){
                    count++;
                    if(count>half_length){
                        list.add(nums[i]);
                    }
                }
            }
        }
       return list.get(0).intValue();
    }
}
```

但这种方法简单粗暴，好费时间

## 3.优化算法(排序取中项法)

排序取中项法相对于前一个方法来说进步了一些，其使用的原理是：

- 在一个排好序的数组中，其最中间的元素必定是多数元素（如果有的话）

![](https://img2018.cnblogs.com/blog/1514171/201811/1514171-20181101214455884-710934253.png)

但这种算法也有其劣势，就是它要求数组进行排序，在数组长度较大的时候，其计算代价也是较大的

## 4.剔除元素法（推荐方法）

因此我们使用推荐的算法，这个算法基于的原理是：

- 在原序列中去除两个不同的元素后，原序列中的多数元素在新序列中还是多数元素。

![](https://img2018.cnblogs.com/blog/1514171/201811/1514171-20181101215031165-84106846.png)

那么问题就简单了，我们只需要将数组中不相同的元素两两剔除，那么剩下的数就自然是多数元素。

我们可以将数组的第一个元素设置为多数元素候选c，以及计数器count = 1，将数组后面的数与c比较，若相等count+1，不相等count-1，若count减到0，便表示该元素出现次数太少，将其和后面一个不相等的元素剔除，再次重置c和count，继续此操作，直到比较到最后count都不为0，则c就为多数元素。

这种算法将复杂过程简化成了两数的比较，大大提升了计算效率。



## 5.代码

```
//寻找多数元素算法练习
public class Marjority {
    private int a[] = new int[50];    //定义存放数据的数组
    public int length;        //定义数组长度
    public Marjority(int length,int a[])
    {
        for(int i=1;i<=length;i++)
            this.a[i] = a[i-1];    //转存数组
            /*需要注意的是，我们的算法中数组下标从1开始
             * 但java存储时的数组下标从0开始，所以转存时下标需-1*/
        this.length = length;
    }
    //寻找多数元素，若存在则输出，不存在返回-99999
    public int Mar()
    {
        int c = candidate(1);
        //进行多数元素查询
        int count = 0;
        for(int j=1;j<=length;j++)
            if(a[j] == c)
                count++;    //计算该元素在数组中出现的次数
        if(count > length/2)    //判断出现次数是否大于长度的一半
            return c;    //大于则有多数元素，返回其值
        else return -9999;    //不大于则没有多数元素，返回-9999
    }
    public int candidate(int n)
    {
        int j = n, c = a[n], count = 1;
        //声明数组索引，当前元素值，计数器
        while(j<length && count>0)
        {
            j++;    //指针向后移动一位
            if(a[j] == c)    //若下一位与当前元素相同，计数器增加
                count++;
            else count--;    //否则计数器减少
            /*这里不太好理解的是计数器的作用，
             * 实际上计数器是一个筛选作用，
             * 当当前元素重复出现的次数太少时，
             * 将这个元素剔除出候选*/
        }
        if(j == length)
            return c;    /*当指针指向最后一个元素时，
                           表示当前元素出现次数足够多，
                           返回其值*/
        else return candidate(j+1);    //否则继续向后遍历
    }
}
```

