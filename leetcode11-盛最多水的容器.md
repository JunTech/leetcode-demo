# leetcode11-[盛最多水的容器](https://leetcode-cn.com/problems/container-with-most-water/)

`类型：数组`

## 1.题目描述

给定 n 个非负整数 a1，a2，...，an，每个数代表坐标中的一个点 (i, ai) 。在坐标内画 n 条垂直线，垂直线 i 的两个端点分别为 (i, ai) 和 (i, 0)。找出其中的两条线，使得它们与 x 轴共同构成的容器可以容纳最多的水。

说明：你不能倾斜容器，且 n 的值至少为 2。

![img](https://aliyun-lc-upload.oss-cn-hangzhou.aliyuncs.com/aliyun-lc-upload/uploads/2018/07/25/question_11.jpg)

图中垂直线代表输入数组 [1,8,6,2,5,4,8,3,7]。在此情况下，容器能够容纳水（表示为蓝色部分）的最大值为 49。

 

示例:

输入: [1,8,6,2,5,4,8,3,7]
输出: 49

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/container-with-most-water
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。



## 2.解题思路

### 2.1 两次循环遍历

初始化一个max,然后在循环中初始化一个临时变量用来存储每一次的最大面积，与max比较，max取比较后的最大值,`area = min(h[i],h[j])×(j−i)`

代码：

```
class Solution {
    public int maxArea(int[] height) {
        int max = 0;
        //下标相减*v
        for (int i = 0; i <height.length ; i++) {
            for (int j = height.length-1; j >i ; j--) {
                //获取下标长度
                int len = j-i;
                int temp = len * (height[j]>height[i]?height[i]:height[j]);
                if(temp >max){
                    max = temp;
                }
            }
        }

        return max;
    }
}
```

执行结果：

```
执行用时 :
302 ms
, 在所有 java 提交中击败了
16.35%
的用户
内存消耗 :
40.3 MB
, 在所有 java 提交中击败了
90.15%
的用户
```

可以看到这种方法是最简单暴力的，但是耗时也是比较夸张的，不推荐

### 2.2双指针

```
public int maxArea(int[] height) {
        int max = 0;
        for(int i =0, j = height.length-1; i < j;){
            int x = j-i;
            int y = height[i] > height[j] ? height[j--] : height[i++];
            if(x*y > max){
                max = x*y;
            } 
        }
        return max;
    }

```

![TIM截图20191218151928.png](https://pic.leetcode-cn.com/e7fef94ca9bb6ff17f84e641ef559a73481551ff8309dd726f21b100f94035b1-TIM%E6%88%AA%E5%9B%BE20191218151928.png)