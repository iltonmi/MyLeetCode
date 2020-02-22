# 334. Increasing Triplet Subsequence

 https://leetcode.com/problems/increasing-triplet-subsequence/discuss/79004/Concise-Java-solution-with-comments. 

>  in this problem we just need to determine whether the subsequence exists. so after assigning the value to `min` and `secondMin`, even though there might be a smaller value afterward and the variable `min` gets updated, it does not affect the `increasing subsequence` overall as long as there is an integer that is larger than 'secondMin' 

上面的话解释了，为什么下面这种情况也是合理的。

>  **[8,7,6,7,5,8]** ， big = 7 small = 5 if any number > big, it's greater than small as well. 

1. one pass，空间O(1)，时间O(N)。

   ```java
   public boolean increasingTriplet(int[] nums) {
       int min = Integer.MAX_VALUE;
       int secondMin = Integer.MAX_VALUE;
       for(int num : nums){
           if(num <= min) {
               min = num;
           } else if(num < secondMin) {
               secondMin = num;
           } else if(num > secondMin) {
               return true;
           }
       }
       return false;
   }
   ```

   