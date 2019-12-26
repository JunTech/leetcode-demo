# leetcode01-两数和

## 1.问题描述

```
给定一个整数数组 nums 和一个目标值 target，请你在该数组中找出和为目标值的那 两个 整数，并返回他们的数组下标。

你可以假设每种输入只会对应一个答案。但是，你不能重复利用这个数组中同样的元素。

示例:

给定 nums = [2, 7, 11, 15], target = 9

因为 nums[0] + nums[1] = 2 + 7 = 9
所以返回 [0, 1]

```

## 2.解题思路

### 2.1暴力解决(o(n2))

采用2个for循环遍历，找到符合条件的值的下标

代码如下：

```
    public int[] twoSum(int[] nums, int target) {

        for (int i = 0; i < nums.length; i++) {
            for (int j = i + 1; j < nums.length; j++) {
                int num = target - nums[i];
                if (nums[j] == num) {
                    return new int[]{i, j};
                }
            }
        }

        return null; 
        
    }
```

运行结果：

```
执行用时 :
23 ms
, 在所有 java 提交中击败了
47.47%
的用户
内存消耗 :
37.4 MB
, 在所有 java 提交中击败了
89.37%
的用户
```

可以看到我们这里运行时间为23ms，用时还是挺久的.


### 2.2 使用hashmap(o(1))

采用hashmap优化一下代码,因为他的时间复杂度为o(1);

代码：

```
        public int[] twoSum(int[] nums, int target) {
        Map<Integer, Integer>  map = new HashMap<Integer, Integer>(10);
        for (int i = 0; i <nums.length ; i++) {
            if(map.containsKey(target-nums[i])){
                return new int[]{map.get(target-nums[i]),i};
            }
            map.put(nums[i],i); //不存在就put
        }
        throw new IllegalArgumentException("没有2个数的和！");
    }
```

```
执行结果：
通过
显示详情
执行用时 :
3 ms
, 在所有 java 提交中击败了
97.84%
的用户
内存消耗 :
37.4 MB
, 在所有 java 提交中击败了
89.05%
的用户
```

只用了3ms,时间效率大大提升。

## 3.潜在问题

可能存在hash碰撞，但是由于题中说明：`可以假设每种输入只会对应一个答案。但是，你不能重复利用这个数组中同样的元素`

所以不用考虑