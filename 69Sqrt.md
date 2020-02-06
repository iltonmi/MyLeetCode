# 69. Sqrt(x)

麻雀虽小五脏俱全。

**基本情况：**0和1的平方根等于自身。

**值得注意的地方：**mid * mid 太多导致溢出，lo + hi 太大导致溢出。

**核心思想：**从[0, x/2+1]中二分查找一个数满足 mid * mid <= x 和 (mid + 1) * (mid + 1) > x。

**为什么**是 mid * mid <= x 和 (mid + 1) * (mid + 1) > x ？

1. 若x的算术平方根是整数，显然成立。
2. 若x的算数平方根是无限小数，以8的平方根是2.83举例，2 * 2 = 4 < 8，3 * 3 = 9 > 8。



**解法：**

1.  二分查找，空间O(1)，时间O(N)

   ```java
   public int mySqrt(int x) {
       if(x == 1 || x == 0) {
           return x;
       }
       int lo = 0;
       int hi = x / 2 + 1;
       int mid = 0;
       while(lo <= hi) {
           //防止lo + hi溢出
           mid = lo + (hi - lo) / 2;
           //防止 mid^2和(mid+1)^2溢出
           if(mid <= x / mid && mid + 1 > x / (mid + 1)) {
               return mid;
           } else if(mid < x / mid) {
               lo = mid + 1;
           } else {
               hi = mid - 1;
           }
       }
       return mid;
   }
   ```

   

2. 牛顿迭代，面试复习中，押后。