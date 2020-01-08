# leetcode[905. 按奇偶排序数组](https://leetcode-cn.com/problems/sort-array-by-parity/)

`类型：算法`

## 1.问题描述

```
给定一个非负整数数组 A，返回一个数组，在该数组中， A 的所有偶数元素之后跟着所有奇数元素。

你可以返回满足此条件的任何数组作为答案。

 

示例：

输入：[3,1,2,4]
输出：[2,4,3,1]
输出 [4,2,3,1]，[2,4,1,3] 和 [4,2,1,3] 也会被接受。
 

提示：

1 <= A.length <= 5000
0 <= A[i] <= 5000

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/sort-array-by-parity
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```

## 2.解题思路

创建一个新的数组保存排序后的数组，将奇数放在后面的索引位置，将偶数放在前面的索引位置，判断奇数的索引位置和偶数的索引位置大小

代码：

```
class Solution {
    public int[] sortArrayByParity(int[] A) {
        int[] a = new int[A.length];
        int endIndex = A.length-1;
        int startIndex = 0;
        if(A.length>=1&&A.length<=5000){
            for (int i = 0; i <A.length ; i++) {
                if(A[i]>=0&&A[i]<=5000&&startIndex<=endIndex){
                    if(A[i]%2==0){
                       a[startIndex] = A[i];
                        startIndex++;
                    }else {
                        a[endIndex] = A[i];
                        endIndex--;
                    }

                }else{
                    throw  new RuntimeException("数据不在可接受范围内！");
                }
            }
        }else {
            throw  new RuntimeException("数据不在可接受范围内！");
        }
        return a;
    }
}
```

