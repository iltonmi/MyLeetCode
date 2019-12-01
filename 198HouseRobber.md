#  198. House Robber 

1. DP，时间O(n), 空间O(1)

   网上一般说法：

   ​	
   $$
   f(k) = 从前 k 个房屋中能抢劫到的最大数额，A_i = 第 i 个房屋的钱数。
   $$
   $$
   f(k) = Max(f(k – 2) + A_k, f(k – 1))
   $$

   **我的理解:**

   1. 增加两个概念：
      $$
      rob[k]=抢劫第k个房屋的情况下, 从前 k 个房屋中能抢劫到的最大数额
      $$

      $$
      notrob[k]=不抢劫第k个房屋的情况下, 从前 k 个房屋中能抢劫到的最大数额
      $$

   2. 显然：
      $$
      f(k) = Max(rob[k], notrob[k])
      $$

   3. 对于notrob[k]:
      $$
      notrob[k]=Max(rob[k-1], notrob[k-1])=f(k-1)
      $$

      $$
      注：f(k-2)<=f(k-1)
      $$

   4. 对于rob[k]:
      $$
      notrob[k]=notrob[k-1]+A_k=f(k-2)+A_k
      $$

   5. 所以有：
      $$
      f(k) = Max(rob[k], notrob[k])=Max(f(k – 2) + A_k, f(k – 1))
      $$

   ```java
   //f(k) = Max(f(k – 2) + A_k, f(k – 1))
   public int rob(int[] nums) {
       int pre = 0;
       int res = 0;
       for(int i = 0; i < nums.length; i++) {
           int tmp = res;
           res = Math.max(pre + nums[i], res);
           pre = tmp;
       }
       return res;
   }
   ```

   ```java
   //f(k) = Max(rob[k], notrob[k])
   public int rob(int[] num) {
       //max monney can get if rob current house
       int rob = 0; 
       //max money can get if not rob current house
       int notrob = 0; 
       for(int i=0; i<num.length; i++) {
           //if rob current value, previous house must not be robbed
           int currob = notrob + num[i]; 
           //if not rob ith house
           //take the max value of robbed (i-1)th house and not rob (i-1)th house
           notrob = Math.max(notrob, rob); 
           rob = currob;
       }
       return Math.max(rob, notrob);
   }
   ```

