#  300. Longest Increasing Subsequence

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

    https://www.geeksforgeeks.org/longest-monotonically-increasing-subsequence-size-n-log-n/ 
   
   当我们遍历数组时，设当前元素 cur，若此前发现了若干个不同长度的递增子序列（相同长度的，只保存变长的可能性更大的一个），则分别比较 cur 和每个递增子序列的最大元素：

   1. 若 cur 比目前最长的递增子序列的末尾元素更大，则说明出现了更长的递增子序列，这个新的子序列由原来最长的子序列和 cur 组成。
   
   2. 若 cur 处于某2个递增子序列的末尾元素之间，将二者之中更长的子序列替换成更短的子序列与 cur 组成的新的序列。因为同样长的子序列中末尾元素越小，在后面变长的可能性越大。
   
   3. 若 cur 比长度为1的递增子序列的元素还小，那么用其替换此元素。原因同2。
   
      核心思想：相同长度的子序列，保存可能变得更长的一个。
   
   ​	上面这个得到最长递增子序列的过程，最重要的操作是在所以队列的末尾元素中二分查找 cur 应该出现的位置。
   
   ​	如果我们只关注长度，而不关注最长递增序列的全貌，那么我们是否可以只保存所有子序列的末尾元素呢？
   
   ​	试想一下，将所有子序列左对齐，将所有末尾元素垂直向下映射到数组中。这样能够在不关注最长递增子序列全貌的情况下，正确地比较、替换和插入末尾元素。
   
   ​	最关键的地方是，末尾元素的个数正好就是最长递增子序列的长度。
   
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
   
   