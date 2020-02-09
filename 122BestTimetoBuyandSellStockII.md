#  122. Best Time to Buy and Sell Stock II		

1. 对于每个波谷，找到后续对应的波峰。

   ```java
   public int maxProfit(int[] prices) {
       int lo = Integer.MAX_VALUE;
       int profit = 0;
       for(int i = 0; i < prices.length; i++) {
           if(prices[i] < lo) {
               //通过对比今天和此前的价格判断波谷
               lo = prices[i];
           } else if(i + 1 == prices.length 
                     || prices[i] > prices[i+1]) {
               //通过对比当天和明天的价格判断波峰
               //注意，‘明天’可能越界
               profit += prices[i] - lo;
               lo = prices[i];
           }
       }
       return profit;
   }
   ```

   

2. 贪心，在上升期每天买卖。

   ```java
   public int maxProfit(int[] prices) {
       int total = 0;
       for (int i=0; i< prices.length-1; i++) {
           if (prices[i+1]>prices[i]) {
               total += prices[i+1]-prices[i];
           }
       }
       
       return total;
   }
   ```

   