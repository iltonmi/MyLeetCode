# 53. Maximum Subarray

1. DP

   this problem was discussed by Jon Bentley (Sep. 1984 Vol. 27 No. 9 Communications of the ACM P885)

   

   the paragraph below was copied from his paper (with a little modifications):

   algorithm that operates on arrays: it starts at the left end (element A[1]) and scans through to the right end (element A[n]), keeping track of the maximum sum subvector seen so far. The maximum is initially A[0]. Suppose we've solved the problem for A[1 .. i - 1]; how can we extend that to A[1 .. i]? The maximum sum in the first I elements is either the maximum sum in the first i - 1 elements (which we'll call MaxSoFar), or it is that of a subvector that ends in position i (which we'll call MaxEndingHere).

   

   MaxEndingHere is either A[i] plus the previous MaxEndingHere, or just A[i], whichever is larger.

   ```java
   public static int maxSubArray(int[] A) {
       int maxSoFar=A[0], maxEndingHere=A[0];
       for (int i=1;i<A.length;++i){
       	maxEndingHere= Math.max(maxEndingHere+A[i],A[i]);
       	maxSoFar=Math.max(maxSoFar, maxEndingHere);	
       }
       return maxSoFar;
   }
   ```

   

2. D&C

   ```java
   class Solution {
       public int maxSubArray(int[] nums) {
           return dac(nums, 0, nums.length - 1);
       }
       
       private int dac(int[] nums, int low, int high) {
           if(low == high) {
               return nums[low];
           }
           int mid = (low + high) / 2;
           return Math.max(Math.max(dac(nums, low, mid), dac(nums, mid+1, high)),
                          maxCrossMiddle(nums, low, high, mid));
       }
       
       private int maxCrossMiddle(int[] nums, int low, int high, int mid) {
           int sum = 0;
           int leftSum = Integer.MIN_VALUE;
           int rightSum = Integer.MIN_VALUE;
           for(int i = mid; i >= low; i--) {
               sum += nums[i];
               leftSum = Math.max(leftSum, sum);
           }
           sum = 0;
           for(int i = mid + 1; i <= high; i++) {
               sum += nums[i];
               rightSum = Math.max(rightSum, sum);
           }
           return leftSum + rightSum;
       }
   }
   ```

   