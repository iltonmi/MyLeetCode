#  152. Maximum Product Subarray 

这道题的3种典型题解都是时间O(N)，空间O(1)，其中主要存在着2种不同的分析角度。

1. 核心逻辑是根据数组是否包含0和数组中负数的个数进行分类讨论。

   根据数组是否包含0进行分类讨论：

   1. 数组不包含0

      在数组不包含0的前提下，又可以根据数组包含负数的奇偶性进行分类：

      1. 若数组不包含0，且包含负数的个数奇数：数组的最大累乘积只有2个可能，以最中间的负数为分界的两个子数组的累乘积中的更大者。
      2. 若数组包含0，且包含负数的个数是偶数：数组的最大累乘积就是整个数组的最大累乘积。

   2. 数组包含0

      在这种情况下，我们将数组以所有的0为界分成多个子数组，这些子数组适用于数组不包含0的情况。

      那么，最大累乘积只可能是0或子数组的所有最大累乘积中的最大的值。

   代码如下：

   ```java
   class Solution {
       public int maxProduct(int[] A) {
           int n = A.length, res = A[0];
           for (int i = 0, l = 0, r = 0; i < n; i++) {
               //当l为0，说明上一个值是0，此时开始计算新的子数组的最大累乘积
               l =  (l == 0 ? 1 : l) * A[i];
               r =  (r == 0 ? 1 : r) * A[n - 1 - i];
               res = Math.max(res, Math.max(l, r));
           }
           return res;
       }
   }
   ```

   ```java
   class Solution {
       public int maxProduct(int[] nums) {
           int firstPass = 1;
           int secondPass = 1;
           int res = Integer.MIN_VALUE;
           
           for(int i = 0; i < nums.length; i++) {
               if(nums[i] == 0) {
                   firstPass = 1;
                   if(res < 0) {
                       res = 0;
                   }
                   continue;
               }
               firstPass *= nums[i];
               res = Math.max(firstPass, res);
           }
           
           for(int i = nums.length - 1; i >= 0; i--) {
               if(nums[i] == 0) {
                   secondPass = 1;
                   if(res < 0) {
                       res = 0;
                   }
                   continue;
               }
               secondPass *= nums[i];
               res = Math.max(secondPass, res);
           }
           return res;
       }
   }
   ```

   

2. 核心逻辑是利用以前一个数结尾的最大累乘数组和最小累乘数组。

   分别设max[i]和min[i]为以nums[i]结尾的数组的最大累乘积和最小累乘积。

   那么nums[i]的值有三个可能性：nums[i], nums[i] * max[i - 1], nums[i] * min[i - 1]

   问题在于，如果nums[i] = nums[i] * max[i - 1]，或nums[i] = nums[i] * min[i - 1]，就有**两个疑问**：

   假设max[i - 1]是nums[k ... i - 1]的累乘积，那nums[i]有没有可能是nums[i] * max[i - 1] * nums[k - 1]呢？

   假设min[i - 1]是nums[k ... i - 1]的累乘积，那nums[i]有没有可能是nums[i] * min[i - 1] * nums[k - 1]呢？

   **答案**：不可能，因为nums[k - 1]的存在能引起max[i - 1]或min[i - 1]的变化，这和max[i - 1]以及min[i - 1]的定义矛盾。

   ```java
   class Solution {
       public int maxProduct(int[] nums) {
           if(nums == null || nums.length == 0) {
               return 0;
           }
           int max = nums[0];
           int min = nums[0];
           int res = nums[0];
           for(int i = 1; i < nums.length; i++) {
               int preMax = max;
               int preMin = min;
               max = Math.max(Math.max(preMax * nums[i], preMin * nums[i]), nums[i]);
               min = Math.min(Math.min(preMax * nums[i], preMin * nums[i]), nums[i]);
               res = Math.max(res, max);
           }
           return res;
       }
   }
   ```

   


