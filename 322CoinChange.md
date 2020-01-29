#  322. Coin Change 

 https://leetcode.com/articles/coin-change/ 

1. 暴力，空间O(N)，时间(S^N)。S为目标值，N为不同面额的硬币种类数量。

   遍历不同个数当前面额的硬币（从0开始），以下一种面额的硬币为参数递归调用。

   ```java
   class Solution {
       private int[] coins;
       public int coinChange(int[] coins, int amount) {
           this.coins = coins;
           return help(amount, 0);
       }
       
       private int help(int remain, int from) {
           if(remain == 0) {
               return 0;
           }
           if(from < coins.length && remain > 0) {
               int minCost = Integer.MAX_VALUE;
               int maxCurrCoin = remain / coins[from];
               for(int i = 0; i <= maxCurrCoin; i++) {
                   if(remain >= i * coins[from]) {
                       int restMinCost = help(remain - i * coins[from], from + 1);
                       if(restMinCost != -1) {
                           minCost = Math.min(minCost, restMinCost + i);
                       }
                   }
               }
               return (Integer.MAX_VALUE == minCost) ? -1 : minCost;
           }
           return -1;
       }
   }
   ```

   

2. DP自顶向下，空间O(S)，时间(S * N)。

   ```java
   class Solution {
       private int[] coins;
       public int coinChange(int[] coins, int amount) {
           this.coins = coins;
           return help(amount, new int[amount]);
       }
       
       private int help(int remain, int[] count) {
           if(remain < 0) {
               return -1;
           } else if(remain == 0) {
               return 0;
           }
           if(count[remain - 1] != 0) {
               return count[remain - 1];
           }
           int min = Integer.MAX_VALUE;
           for(int coin : coins) {
               int restMinCost = help(remain - coin, count);
               if(restMinCost >= 0 && restMinCost < min) {
                   //f(s) = f(s-c) + 1
                   min = restMinCost + 1;
               }
           }
           count[remain - 1] = (min == Integer.MAX_VALUE) ? -1 : min;
           return count[remain - 1];
       }
   }
   ```

   

3. DP自底向上，空间O(S)，时间(S * N)。

   ```java
   class Solution {
       public int coinChange(int[] coins, int amount) {
           int[] dp = new int[amount + 1];
           //默认值不要选择Integer.MAX_VALUE，否则可能溢出
           Arrays.fill(dp, amount + 1);
           dp[0] = 0;
           for(int i = 1; i <= amount; i++) {
               for(int j = 0; j < coins.length; j++) {
                   if(coins[j] <= i) {
                       //这里，若dp[i - coins[j]]的默认值是MAX_VALUE，会溢出
                       dp[i] = Math.min(dp[i], dp[i - coins[j]] + 1);
                   }
               }
           }
           return dp[amount] > amount ? -1 : dp[amount];
       }
   }
   ```

   