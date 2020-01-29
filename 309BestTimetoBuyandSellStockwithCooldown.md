#  309. Best Time to Buy and Sell Stock with Cooldown 

1. 暴力递归（超时），空间O(N)，时间(2^N)

   ```java
   class Solution {
       int[] prices;
       public int maxProfit(int[] prices) {
           this.prices = prices;
           return help(0, null, true, false);
       }
       
       private int help(int cur, Integer buyDay, boolean canBuy, boolean coolDown) {
           if(cur == prices.length) {
               return 0;
           }
           if(coolDown) {
               return help(cur + 1, null, true, false);
           } else if(canBuy) {
               int buy = help(cur + 1, cur, false, false);
               int notbuy = help(cur + 1, null, true, false);
               return Math.max(buy, notbuy);
           } else {
               int sold = prices[cur] - prices[buyDay] 
                   + help(cur + 1, null, false, true);
               int notsold = help(cur + 1, buyDay, false, false);
               return Math.max(sold ,notsold);
           }
       }
   }
   ```

   

2. 动态规划，空间O(N)，时间(N)。状态的定义太难了，很无语，DP还是得多做。

   这种含有状态变化的dp，不像纯粹的买卖，带有买和卖的限制，所有就可以尝试定义2个数组，一个和买有关，一个和卖有关。

   参考：https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/discuss/75931/Easiest-JAVA-solution-with-explanations

   the definitions of `buy[i]` and `sell[i]` can be refined to these:

   - `buy[i]` : Maximum profit which end with buying on day `i` or end with buying on a day before `i` and takes rest until the day `i` since then.
   - `sell[i]` : Maximum profit which end with selling on day `i` or end with selling on a day before `i` and takes rest until the day `i` since then.

   ```java
   class Solution {
       public int maxProfit(int[] prices) {
           if(prices == null || prices.length == 0) {
               return 0;
           }
           //buy[i] = max(sell[i-2]-prices[i], buy[i-1]);
           //sell[i] = max(sell[i-1], buy[i-1]+prices[i]);
           int si2 = 0;
           int si1 = 0;
           int bi1 = -prices[0];
           for(int i = 1; i < prices.length; i++) {
               int temp = si1;
               si1 = Math.max(si1, bi1 + prices[i]);
               bi1 = Math.max(si2 - prices[i], bi1);
               si2 = temp;
           }
           return si1;
       }
   }
   ```