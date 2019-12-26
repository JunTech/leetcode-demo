# leetcode07-整数反转

## 1.问题描述

```
给出一个 32 位的有符号整数，你需要将这个整数中每位上的数字进行反转。

示例 1:

输入: 123
输出: 321
 示例 2:

输入: -123
输出: -321
示例 3:

输入: 120
输出: 21

```

## 2.解题思路

要判断溢出，Integer的最大值，和最小值以及如何判断临界值，然后一步步除余，在反转

代码如下：

```
// class Solution {
//     public int reverse(int x) {
//         int y = 0;
//         while (x != 0) {
//             if (y > 214748364 || y < -214748364) {
//                 return 0;
//             }
//             y = y * 10 + x % 10;
//             x = x / 10;
//         }
//         return y;
//     }
// }

class Solution {
    public int reverse(int x) {
        int result = 0;
        while (x != 0) {
            if (result>Integer.MAX_VALUE/10 ||(result == Integer.MAX_VALUE/10 && x%10>Integer.MAX_VALUE%10)){
                return 0;
            }
            if(result < Integer.MIN_VALUE/10 || (result == Integer.MIN_VALUE/10 && x%10<Integer.MIN_VALUE%10)){
                return 0;
            }
            result = result*10+x%10;
            x = x /10;

        }
        return result;
    }
}
```

耗时：1ms 占用内存：33.4M

可以看到效率挺高的