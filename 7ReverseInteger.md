#  7. Reverse Integer

这个问题最大的意义在于，**处理溢出的问题**。

第1种方法需要记住 int 的最大值和最小值的最后一位。第2种方法在C中会抛出异常。

1. 判断**下一个值是否溢出**

   To explain, lets assume that rev is positive.

   1. **If rev > Integer.MAX_VALUE/10**，rev = rev * 10 will overflow, so rev = **rev * 10 + pop must overflow**.

   2. **If rev == Integer.MAX_VALUE/10**, whether rev * 10 + pop will overflow **depends on pop.**

   3. reference:

      > Integer.MAX_VALUE = 2147483647
      > Integer.MIN_VALUE = -2147483648

   ```java
   class Solution {
       public int reverse(int x) {
           int rev = 0;
           while (x != 0) {
               int pop = x % 10;
               x /= 10;
               if (rev > Integer.MAX_VALUE/10 
                   || (rev == Integer.MAX_VALUE / 10 && pop > 7)) {
                   return 0;
               }
               if (rev < Integer.MIN_VALUE/10 
                   || (rev == Integer.MIN_VALUE / 10 && pop < -8)) {
                   return 0;
               }
               rev = rev * 10 + pop;
           }
           return rev;
       }
   }
   ```

2. 判断**新的值是否溢出**

   **判断依据：新的值是否等于原来的值。**

   ```java
   class Solution {
       public int reverse(int x) {
           int res = 0;
   
           while (x != 0) {
               int tail = x % 10;
               int next = res * 10 + tail;
               if ((next - tail) / 10 != res) {	
                   return 0;
               }
               res = next;
               x /= 10;
           }
   
           return res;
       }
   }
   ```

   