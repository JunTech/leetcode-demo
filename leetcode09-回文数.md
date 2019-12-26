# leetcode09-回文数

## 1.问题描述

```
判断一个整数是否是回文数。回文数是指正序（从左向右）和倒序（从右向左）读都是一样的整数。

示例 1:

输入: 121
输出: true
示例 2:

输入: -121
输出: false
解释: 从左向右读, 为 -121 。 从右向左读, 为 121- 。因此它不是一个回文数。
示例 3:

输入: 10
输出: false
解释: 从右向左读, 为 01 。因此它不是一个回文数。

```

## 2.解题思路

​	首先小于0的不用考虑，不可能是回文数，0是，其余大于0的数采用第07题的数字反转思想，除余获取反转后的数，在比较即可

代码如下

```
class Solution {
    public boolean isPalindrome(int x) {
        int result =0;
        int y = x;
        if(x<0){
            return false;
        }else if(x==0){
            return true;
        }else {
            while (x!=0){
                result = result*10+x%10;
                x/=10;
            }
            if(y == result){
                return true;
            }else {
                return false;
            }
        }
    }
}


```

结果：

```
执行用时 :
9 ms
, 在所有 java 提交中击败了
98.60%
的用户
内存消耗 :
36.8 MB
, 在所有 java 提交中击败了
91.80%
的用户
```



