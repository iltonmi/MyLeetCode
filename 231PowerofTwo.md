#  231. Power of Two 

集邮，加强位运算技巧的应用。

1. 常规

   ```java
   public boolean isPowerOfTwo(int n) {
       if (n <= 0) {
           return false;
       }
       while (n % 2 == 0) {
           n /= 2;
       }
       return n == 1;
   }
   ```

   

2. 位运算，保留最右边的1

   如果是2的幂，它的二进制只有一个位置是1

   所以，它最右边的1等于它本身

   ```java
   public boolean isPowerOfTwo(int n) {
       if (n <= 0) {
           return false;
       }
       return (n & (-n)) == n;
   }
   ```

   

3. 位运算，去除（抹掉）最右边的1

   如果是2的幂，它的二进制只有一个位置是1

   所以它去除最右边的1之后等于0

   ```java
   public boolean isPowerOfTwo(int n) {
       if (n <= 0) {
           return false;
       }
       return (n & (n - 1)) == 0;
   }
   ```

   