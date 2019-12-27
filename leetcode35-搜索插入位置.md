# leetcode35-[搜索插入位置](https://leetcode-cn.com/problems/search-insert-position/)

`类型：数组`

## 1.题目描述

```
给定一个排序数组和一个目标值，在数组中找到目标值，并返回其索引。如果目标值不存在于数组中，返回它将会被按顺序插入的位置。

你可以假设数组中无重复元素。

示例 1:

输入: [1,3,5,6], 5
输出: 2
示例 2:

输入: [1,3,5,6], 2
输出: 1
示例 3:

输入: [1,3,5,6], 7
输出: 4
示例 4:

输入: [1,3,5,6], 0
输出: 0

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/search-insert-position
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```

## 2.解题思路

代码：

```
class Solution {
     public int searchInsert(int[] nums, int target) {
        if(nums[0]>=target){
            return 0;
        }
        if(target>nums[nums.length-1]){
            return nums.length;
        }

        int res = 0;
        for (int i = 0; i < nums.length-1; i++) {
            if(target<=nums[i+1]&&target>nums[i]){
                res = i+1;
            }
        }

        return res;
    }
}
```

执行结果：

```
执行用时 :
0 ms
, 在所有 java 提交中击败了
100.00%
的用户
内存消耗 :
38.8 MB
, 在所有 java 提交中击败了
58.74%
的用户
```

| 提交时间 | 提交结果 | 执行用时 | 内存消耗 | 语言 |
| -------- | -------- | -------- | -------- | ---- |
| 几秒前   | [通过]() | 0 ms     | 38.8 MB  | Java |