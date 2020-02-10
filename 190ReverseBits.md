#  190. Reverse Bits

1. 直接，空间O(1)，时间O(N)。

   ```java
   // you need treat n as an unsigned value
   public int reverseBits(int n) {
       int res = 0;
       for(int i = 0; i < 32; i++) {
           int lastBit = n & 1;
           n >>>= 1;
           res = (res << 1) + lastBit;
       }
       return res;
   }
   ```

   

2. 超自然算法，分而治之，空间O(1)，时间O(1)。

   这几个数字，也可以用来，分而治之计算1的个数。

   ```java
   // you need treat n as an unsigned value
   public int reverseBits(int n) {
       int ret=n;
       ret = ret >>> 16 | ret<<16;
       ret = (ret & 0xff00ff00) >>> 8 | (ret & 0x00ff00ff) << 8;
       ret = (ret & 0xf0f0f0f0) >>> 4 | (ret & 0x0f0f0f0f) << 4;
       ret = (ret & 0xcccccccc) >>> 2 | (ret & 0x33333333) << 2;
       ret = (ret & 0xaaaaaaaa) >>> 1 | (ret & 0x55555555) << 1;
       return ret;
   }
   ```

   

