#  （mark）300. Longest Increasing Subsequence

 https://leetcode.com/articles/longest-increasing-subsequence/ 

1. 暴力（超时），空间O(N)，时间(2^N)

   ```java
   class Solution {
       public int lengthOfLIS(int[] nums) {
           return help(nums, Integer.MIN_VALUE, 0);
       }
       
       public int help(int[] arr, int preVal, int nowIndex) {
           if(arr == null || nowIndex == arr.length) {
               return 0;
           }
           int taken = 0;
           if(arr[nowIndex] > preVal) {
               taken = help(arr, arr[nowIndex], nowIndex + 1) + 1;
           }
           int notTaken = help(arr, preVal, nowIndex + 1);
           return Math.max(taken, notTaken);
       }
   }
   ```

   

2. 记忆暴力，空间O(N ^ 2)，时间(N ^ 2)

   ```java
   class Solution {
       Integer[][] memo;
       public int lengthOfLIS(int[] nums) {
           if(nums == null) {
               return 0;
           }
           this.memo = new Integer[nums.length][nums.length];
           return help(nums, -1, 0);
       }
       
       public int help(int[] arr, int pre, int cur) {
           if(cur == arr.length) {
               return 0;
           }
           if(memo[cur][pre+1] != null) {
               return memo[cur][pre+1];
           }
           int taken = 0;
           if(pre < 0 || arr[cur] > arr[pre]) {
               taken = help(arr, cur, cur + 1) + 1;
           }
           int notTaken = help(arr, pre, cur + 1);
           memo[cur][pre+1] = Math.max(taken, notTaken);
           return memo[cur][pre+1];
       }
   }
   ```

   

3. DP，空间O(N)，时间(N ^ 2)。

   dp[i] 的定义：包含以 nums[i] 结尾的最长递增子序列。

   补充，若需要还原子序列，只需要额外使用一个数字保存nums[i] 结尾的最长递增子序列中前一个数字的下标。

   ```java
   class Solution {
       public int lengthOfLIS(int[] nums) {
           if(nums == null) {
               return 0;
           }
           int[] dp = new int[nums.length];
           int max = 0;
           for(int i = 0; i < nums.length; i++) {
               for(int j = 0; j < i; j++) {
                   if(nums[i] > nums[j]) {
                       dp[i] = Math.max(dp[i], dp[j]);
                   }
               }
               dp[i]++;
               max = Math.max(max, dp[i]);
           }
           return max;
       }
   }
   ```

   

4. DP+二分查找，空间O(N)，时间(NlogN)。

   这题很明显可以讲清楚怎么做，但很难说。
   
   ```java
class Solution {
       int validLen = 0;
       public int lengthOfLIS(int[] nums) {
           if(nums == null) {
               return 0;
           }
           int[] arr = new int[nums.length+1];
           int rightBound = 0;
           for(int num : nums) {
               int index = binarySearch(arr, num, rightBound);
               arr[index] = num;
               if(index > rightBound) {
                   rightBound++;
               }
           }
           return rightBound;
       }
       
       private int binarySearch(int[] arr, int target, int rightBound) {
           if(arr[rightBound] < target) {
               return rightBound + 1;
           }
           int lo = 1;
           int hi = rightBound;
           while(lo <= hi) {
               int mid = lo + ((hi - lo) >> 1);
               if(arr[mid] < target) {
                   lo = mid + 1;
               } else if(arr[mid] > target) {
                   hi = mid - 1;
               } else {
                   return mid;
               }
           }
           return lo;
       }
   }
   ```
   
   