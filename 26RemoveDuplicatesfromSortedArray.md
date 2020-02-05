# 26. Remove Duplicates from Sorted Array

1. 双指针，慢指针指向当前可能重复的元素，快指针需要被检查的元素。空间O(1)，时间O(N)。

   ```
   class Solution {
       public int removeDuplicates(int[] nums) {
           if(nums.length == 1) {
               return 1;
           }
           int i = 0;
           for(int j = 1; j < nums.length; j++) {
               if(nums[i] != nums[j]) {
                   i++;
                   nums[i] = nums[j];
               }
           }
           return i + 1;
       }
   }
   ```

   