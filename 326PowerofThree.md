#  326. Power of Three

 https://leetcode.com/articles/power-of-three/ 

1. **迭代，等价于另一种做法：转换成3进制**，观察比最高位更低的位置是否都是0。
   $$
   空间O(1)，时间O(log_3N)。
   $$

   ```java
   public boolean isPowerOfThree(int n) {
       if (n < 1) {
           return false;
       }
       while (n % 3 == 0) {
           n /= 3;
       }
       return n == 1;
   }
   ```

   

2. 使用JDK内置函数，配合对数换底公式。空间O(1)，时间未知。

   ```java
   public boolean isPowerOfThree(int n) {
       return (Math.log10(n) / Math.log10(3)) % 1 == 0;
   }
   ```

   

3. （仅供观赏）3的最大幂，整数限制

   整数限制： Integer.MAX_VALUE 
   $$
   3的最大幂=N的限制=3^{log_3(MaxInt)}=3^{19.56} =3^{19}=1162261467
   $$
   **所有3的幂都是3的最大幂的公因数**。

   ```java
   public boolean isPowerOfThree(int n) {
       return n > 0 && 1162261467 % n == 0;
   }
   ```

   