# 338. Counting Bits

1. 遍历，然后请参看另一篇文章: 整数的二进制数表达有多少个1

2. DP。

   原理：i >> 1 的二进制表达相当于 i 的二进制表达右移。i >> 1和 i 的二进制表达中1的数量，取决于i的最低位是否是1。由此可得：
   $$
   f[i] = f[i>>1] + (i\&1);
   $$
   

   ```java
   public int[] countBits(int num) {
       int[] res = new int[num + 1];
       for (int i = 1; i <= num; i++) {
           res[i] = res[i >> 1] + (i & 1);   
       }
       return res;
   }
   ```

   