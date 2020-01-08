# leetcode[88. 合并两个有序数组](https://leetcode-cn.com/problems/merge-sorted-array/)

`类型：算法`

## 1.问题描述

```
给定两个有序整数数组 nums1 和 nums2，将 nums2 合并到 nums1 中，使得 num1 成为一个有序数组。

说明:

初始化 nums1 和 nums2 的元素数量分别为 m 和 n。
你可以假设 nums1 有足够的空间（空间大小大于或等于 m + n）来保存 nums2 中的元素。
示例:

输入:
nums1 = [1,2,3,0,0,0], m = 3
nums2 = [2,5,6],       n = 3

输出: [1,2,2,3,5,6]

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/merge-sorted-array
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```

## 2.解题思路

### 2.1先整合成一个数组在排序

代码如下：

```
class Solution31 {
    public void merge(int[] nums1, int m, int[] nums2, int n) {
      System.arraycopy(nums2,0,nums1,m,n);
        Arrays.sort(nums1);
    }
}
```

| 24 分钟前 | [通过]() | 1 ms | 36.5 MB | Java |
| --------- | -------- | ---- | ------- | ---- |
|           |          |      |         |      |

### 2.2使用双指针解决

代码：

```
class Solution {
    public void merge(int[] nums1, int m, int[] nums2, int n) {
        // System.arraycopy(nums2,0,nums1,m,n);
        // Arrays.sort(nums1);
        //采用双指针
        int[] copynum1 = new int[m];
        //将nums1 复制到 copynum1中
        System.arraycopy(nums1, 0, copynum1, 0, m);
        //定义双指针p1,p2
        int p1 = 0;
        int p2 = 0;

        //设置num1的指针p
        int p = 0;
        //  比较
        while ((p1<m)&&(p2<n)){
            nums1[p++] = (copynum1[p1]>nums2[p2]?nums2[p2++]:copynum1[p1++]);
        }
        if (p1 < m){
            System.arraycopy(copynum1, p1, nums1, p1 + p2, m + n - p1 - p2);
        }

        if (p2 < n){
            System.arraycopy(nums2, p2, nums1, p1 + p2, m + n - p1 - p2);
        }

    }
}
```

| 提交时间 | 提交结果 | 执行用时 | 内存消耗 | 语言 |
| -------- | -------- | -------- | -------- | ---- |
| 几秒前   | [通过]() | 0 ms     | 36.4 MB  | Java |