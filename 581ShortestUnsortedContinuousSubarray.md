# 581. Shortest Unsorted Continuous Subarray

 https://leetcode.com/articles/shortest-unsorted-continous-subarray/ 

1.  暴力枚举每一对i，j，C(N, 2) * N = O(N ^ 3)。

2. 在新数组中排序，对比不一样的位置，时间取决于排序算法，空间O(max(N, 排序空间复杂度))。

3. 难以名状，空间O(1)，时间O(N)

    ![Unsorted_subarray](https://leetcode.com/articles/Figures/581/Unsorted_subarray_2.PNG) 

   ```java
   public int findUnsortedSubarray(int[] nums) {
       if(nums == null || nums.length < 2) {
           return 0;
       }
       int min = nums[nums.length - 1];
       int l = -1;
       //从后往前遍历
       for(int i = nums.length - 1; i != -1; i--) {
           //若nums[i]大于当前被遍历过的值中的最小值，说明nums[i]需要被调整，因为违反了升序
           if(nums[i] > min) {
               l = i;
           } else {
               min = Math.min(min, nums[i]);
           }
       }
       if(l == -1) {
           return 0;
       }
       int max = nums[0];
       int r = -1;
       //从前往后遍历
       for(int i = 1; i != nums.length; i++) {
           //若nums[i]小于当前被遍历过的值中的最大值，说明nums[i]需要被调整，因为违反了升序
           if(nums[i] < max) {
               r = i;
           } else {
               max = Math.max(max, nums[i]);
           }
       }
       return r - l + 1;
   }
   ```

   