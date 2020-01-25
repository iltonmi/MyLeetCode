#  238. Product of Array Except Self

这道题不准用除法，关键在于：

​	如何在计算 nums[i] 时分别知道：左边以及右边的元素的乘积。

计算空间复杂度时，不考虑输出数组。

1. 用两个数组，时间O(N)，空间O(N)。其中：
   $$
   左边元素的乘积：help1[i] = nums[0] * ... * num[i]
   $$

   $$
   右边元素的乘积：help2[i] = nums[i] * ... * nums[length-1]
   $$

   

2. 用一个数组，时间(N)，空间O(1)。其中：
   $$
   左边元素的乘积：help1[i] = nums[0] * ... * num[i]
   $$
   至于右边元素的乘积，用1个变量保存，从右边开始遍历数组时，逐个累乘。

   ```java
   class Solution {
       //let output[i] = nums[0] * ... * nums[i]
       // for i := n to 1, let output[i] = output[i-1] * output[i+1];
       public int[] productExceptSelf(int[] nums) {
           if(nums == null) {
               return null;
           }
           int[] res = new int[nums.length];
           for(int i = 0; i < res.length; i++) {
               res[i] = (i == 0 ? 1 : res[i-1]) * nums[i];
           }
           int R = 1;
           for(int i = res.length - 1; i >= 0; i--) {
               res[i] = (i == 0 ? 1 : res[i-1]) * R;
               R *= nums[i];
           }
           return res;
       }
   }
   ```

   