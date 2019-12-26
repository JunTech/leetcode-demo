# LeetCode-01:只出现一次的数字

## 1.描述

给定一个**非空**整数数组，除了某个元素只出现一次以外，其余每个元素均出现两次。找出那个只出现了一次的元素。

**说明：**

你的算法应该具有线性时间复杂度。 你可以不使用额外空间来实现吗？

**示例 1:**

```
输入: [2,2,1]
输出: 1
```

**示例 2:**

```
输入: [4,1,2,1,2]
输出: 4
```

## 2.解题思路

先将数组转化为list,初始化一个目标index,用于接收一次出现的数字的索引值，循环遍历数组，通过数组中值在list中出现的第一个索引值和最后一个索引值比较，如果相等，说明就是他，如果不等，说明不是，就继续循环

```
class Solution {
    public int singleNumber(int[] nums) {
        List list = new ArrayList(nums.length);
        int target =0;
        for(int num:nums){
            list.add(num);
        }
        for(int i=0;i<nums.length;i++){
            int i1 = list.indexOf(nums[i]);
            int i2 = list.lastIndexOf(nums[i]);
            if(i1 != i2){
                continue;
            }else{
                target = i1;
            }

        }
        return Integer.valueOf(list.get(target).toString());
    }
}
```

```
执行状态：已完成
×
代码执行结果：
我的输入
[2,2,1]
我的答案
1
预期答案
1

```



## 3.耗时最短的解题方法（来源于网上）

```
class Solution {
    public int singleNumber(int[] nums) {
         int index = 0;
        for(int i=0;i<nums.length;i++){       
            index = index ^ nums[i];  
        }
        return index;
    }
}
```

