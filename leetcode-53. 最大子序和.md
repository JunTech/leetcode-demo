# leetcode-[53. 最大子序和](https://leetcode-cn.com/problems/maximum-subarray/)

`类型：动态规划`

## 1.问题描述

```
给定一个整数数组 nums ，找到一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。

示例:

输入: [-2,1,-3,4,-1,2,1,-5,4],
输出: 6
解释: 连续子数组 [4,-1,2,1] 的和最大，为 6。
进阶:

如果你已经实现复杂度为 O(n) 的解法，尝试使用更为精妙的分治法求解。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/maximum-subarray
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```

## 2.解题思路

```
在整个数组或在固定大小的滑动窗口中找到总和或最大值或最小值的问题可以通过动态规划（DP）在线性时间内解决。
有两种标准 DP 方法适用于数组：
常数空间，沿数组移动并在原数组修改。
线性空间，首先沿 left->right 方向移动，然后再沿 right->left 方向移动。 合并结果。
```

代码：

```
class Solution {
    public int maxSubArray(int[] nums) {
        int len = nums.length;
        int maxSum=nums[0],currSum=nums[0];

        for (int i = 1; i <len ; i++) {
            currSum = Math.max(nums[i],currSum+nums[i]);
            maxSum = Math.max(maxSum,currSum);
        }
        return maxSum;
    }
}
```

执行结果：

通过

显示详情

执行用时 :1 ms, 在所有 Java 提交中击败了99.98%的用户

内存消耗 :38.9 MB, 在所有 Java 提交中击败了80.27%的用户