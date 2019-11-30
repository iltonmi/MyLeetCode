#  121. Best Time to Buy and Sell Stock 

1. 使用hashmap, 空间O(n), 时间O(n)

   ```java
   class Solution {
       public int maxProfit(int[] prices) {
           if(prices == null || prices.length < 2) {
               return 0;
           }
           int res = prices[1] - prices[0];
           if (prices.length == 2) {
               return res > 0 ? res : 0;
           }
           Map<Integer, Integer> map = new HashMap<>();
           map.put(0, -1);
           map.put(1, 0);
           for(int i = 2; i < prices.length; ++i ) {
               if(prices[i] > prices[i-1]) {
                   res = Math.max(res,
                               Math.max(prices[i] - prices[i-1],
                                        prices[i] - prices[map.get(i-1)]
                                       )
                              );
                   map.put(i, prices[i] - prices[i-1] > prices[i] - prices[map.get(i-1)] ?
                           i-1 : map.get(i-1)
                          );
               } else {
                   res = Math.max(prices[i-1] - prices[map.get(i-1)] - (prices[i-1] - prices[i]),
                                  res
                                 );
                   map.put(i, map.get(i-1));
               }
           }
           return res > 0 ? res : 0;
       }
   }
   ```

   

2. 暴力
   $$
   O(N^2)
   $$

   ```java
   public class Solution {
      public int maxProfit(int[] prices) {
           if(prices == null || prices.length < 2) {
               return 0;
           }
           int res = 0;
           for(int i = 0; i < prices.length; i++) {
               for(int j = i - 1; j >= 0; j--) {
                   res = Math.max(res, prices[i] - prices[j]);
               }
           }
           return res;
       }
   }
   ```

   

3. #### One Pass

 ![Profit Graph](https://leetcode.com/media/original_images/121_profit_graph.png) 

 The points of interest are the peaks and valleys in the given graph. We need to find the largest peak following the smallest valley. We can maintain two variables - minprice and maxprofit corresponding to the smallest valley and maximum profit (maximum difference between selling price and minprice) obtained so far respectively. 

```java
public int maxProfit(int[] prices) {
        if(prices == null || prices.length < 2) {
            return 0;
        }
        int min = Integer.MAX_VALUE;
        int res = 0;
        for(int i = 0; i < prices.length; i++) {
            if(prices[i] < min) {
                //只需要记录最低价，最大利润不可能是最低价这一天卖出
                min = prices[i];
            } else {
                //更新最大利润
                res = Math.max(res, prices[i] - min);
            }
        }
        return res;
    }
```

