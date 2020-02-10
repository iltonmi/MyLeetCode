#  198. House Robber 

1. 自顶向下递归，空间O(N)，时间O(2 ^ N)。

   ```java
   //递归版本超时1
   class Solution {
       private int[] nums;
       public int rob(int[] nums) {
           this.nums = nums;
           int[] temp = rob(0);
           return Math.max(temp[0], temp[1]);
       }
       
       private int[] rob(int i) {
           int[] res = new int[2];
           if(i >= nums.length) {
               return res;
           }
           int[] temp1 = rob(i+2);
           int[] temp2 = rob(i+1);
           int rob = nums[i] + Math.max(temp1[0], temp1[1]);
           int notrob = Math.max(temp2[0], temp2[1]);
           return Math.max(rob, notrob);
       }
   }
   //递归版本超时2
   class Solution {
       private int[] nums;
       public int rob(int[] nums) {
           this.nums = nums;
           return rob(0);
       }
       
       private int rob(int i) {
           if(i >= nums.length) {
               return 0;
           }
           int rob = nums[i] + rob(i+2);
           int notrob = rob(i+1);
           return Math.max(rob, notrob);
       }
   }
   ```
   
   
   
2. 自顶向上，记忆化递归，空间O(N)，时间O(N)。

   ```java
   //记忆化递归1
   class Solution {
       private int[] nums;
       private Integer[][] map;
       public int rob(int[] nums) {
           this.nums = nums;
           this.map = new Integer[nums.length][2];
           Integer[] temp = rob(0);
           return Math.max(temp[0], temp[1]);
       }
       
       private Integer[] rob(int i) {
           Integer[] res = new Integer[]{0, 0};
           if(i >= nums.length) {
               return res;
           }
           if(map[i][0] != null) {
               return map[i];
           }
           Integer[] temp1 = rob(i+2);
           Integer[] temp2 = rob(i+1);
           Integer rob = nums[i] + Math.max(temp1[0], temp1[1]);
           Integer notrob = Math.max(temp2[0], temp2[1]);
           map[i] = new Integer[]{rob, notrob};
           return map[i];
       }
   }
   
   //记忆化递归2
   class Solution {
       private int[] nums;
       private Integer[] dp;
       public int rob(int[] nums) {
           this.nums = nums;
           dp = new Integer[nums.length];
           return rob(0);
       }
       
       private int rob(int i) {
           if(i >= nums.length) {
               return 0;
           }
           if(dp[i] != null) {
               return dp[i];
           }
           int rob = nums[i] + rob(i+2);
           int notrob = rob(i+1);
           dp[i] = Math.max(rob, notrob);
           return dp[i];
       }
   }
   ```

   

3. 自底向上迭代，空间O(N)，时间O(N)。

   ```java
   public int rob(int[] nums) {
       if(nums == null || nums.length == 0) {
           return 0;
       }
       if(nums.length == 1) {
           return nums[0];
       }
       int[] dp = new int[nums.length];
       dp[0] = nums[0];
       dp[1] = Math.max(nums[0], nums[1]);
       for(int i = 2; i < nums.length; i++) {
           dp[i] = Math.max(dp[i-1], nums[i] + dp[i-2]);
       }
       return dp[dp.length-1];
   }
   ```

4. 自底向上迭代，空间优化，空间O(1)，时间O(N)。

   ```java
   public int rob(int[] nums) {
       if(nums == null || nums.length == 0) {
           return 0;
       }
       if(nums.length == 1) {
           return nums[0];
       }
       int pre = nums[0];
       int cur = Math.max(nums[0], nums[1]);
       for(int i = 2; i < nums.length; i++) {
           int temp = cur;
           cur = Math.max(cur, nums[i] + pre);
           pre = temp;
       }
       return cur;
   }
   ```

   