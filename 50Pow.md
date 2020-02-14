#  50. Pow(x, n)

 https://leetcode-cn.com/problems/powx-n/solution/powx-n-by-leetcode/ 

> 如果 n < 0，我们可以用1 / x, -n 代替 x,n 来保证 n≥0 。这个限制可以简化我们下面的讨论。
>
> 但是我们仍需关注边界条件，尤其是正整数和负整数的不同范围限制。
>

1. 暴力（超时），空间O(1), 时间O(N)

2. 快速幂算法，递归，空间O(logN)，时间O(logN)

   ```java
   public double myPow(double x, int n) {
       //使用long保存n的相反数，如果n是MIN_VALUE可能溢出
       long N = n;
       if (N < 0) {
           x = 1 / x;
           N = -N;
       }
       return fastPow(x, N);
   }
   
   private double fastPow(double x, long n) {
       if (n == 0) {
           return 1.0;
       }
   
       double half = fastPow(x, n >> 1);
       if ((n & 1) == 0) {
           return half * half;
       } else {
           return half * half * x;
       }
   }
   ```

   

3. 快速幂算法，迭代，空间O(1)，时间O(logN)

   乘法分配律，n = (n0 * (2^0) + ... + n31 * (2 ^ 31))，x ^ n = x ^ (n0 * (2^0)) + ... + x ^ (n31 * (2^31))。
   
   ```java
   public double myPow(double x, int n) {
       long N = n;
       if (N < 0) {
           x = 1 / x;
           N = -N;
       }
       double ans = 1;
       double current_product = x;
       for (long i = N; i > 0; i >>= 1) {
           if ((i & 1) == 1) {
               //这个条件只会在第一次通过
               ans = ans * current_product;
           }
           current_product = current_product * current_product;
       }
    return ans;
   }
   ```
   
   