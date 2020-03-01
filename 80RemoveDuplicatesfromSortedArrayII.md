#  80. Remove Duplicates from Sorted Array II

1. 双指针，空间复杂度O(1)，时间复杂度O(N)。

   ```java
   public int removeDuplicates(int[] nums) {
       if(nums == null) {
           return 0;
       }
       if(nums.length < 2) {
           return nums.length;
       }
       int cur = nums[0];
       int res = 1;
       for(int r = 1, l = 0, count = 1; r < nums.length; r++) {
          if(nums[r] == cur && count >= 2) {
               continue;
           }
           nums[++l] = nums[r];
           res++;
           if(nums[r] == cur && count < 2) {
               count++;
           }
           if(nums[r] != cur) {
               cur = nums[r];
               count = 1;
           }
       }
       return res;
   }
   ```

   