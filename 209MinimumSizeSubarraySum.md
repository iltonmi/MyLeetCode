# 209. Minimum Size Subarray Sum

1. 双指针，滑动窗口，空间O(1)，时间O(N)。

   ```java
   public int minSubArrayLen(int s, int[] nums) {
       if(nums == null || nums.length == 0) {
           return 0;
       }
       int sum = 0;
       int res = Integer.MAX_VALUE;
       for(int l = -1, r = 0; r < nums.length; r++) {
           sum += nums[r];
           while(sum >= s) {
               res = Math.min(res, r - l);
               l++;
               sum -= nums[l];
           }
       }
       return res == Integer.MAX_VALUE ? 0 : res;
   }
   ```

   