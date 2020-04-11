#  258. Add Digits 

1. 观察

   > I'll try to explain the math behind this:
   >
   > First you should understand:
   >
   > ```java
   > 10^k % 9 = 1
   > (a * 10^k) % 9 = a % 9
   > ```
   >
   > Then let's use an example to help explain.
   >
   > Say a number x = 23456
   >
   > x = 2* 10000 + 3 * 1000 + 4 * 100 + 5 * 10 + 6
   >
   > 2 * 10000 % 9 = 2 % 9
   >
   > 3 * 1000 % 9 = 3 % 9
   >
   > 4 * 100 % 9 = 4 % 9
   >
   > 5 * 10 % 9 = 5 % 9
   >
   > Then x % 9 = ( 2+ 3 + 4 + 5 + 6) % 9, note that x = 2* 10000 + 3 * 1000 + 4 * 100 + 5 * 10 + 6
   >
   > So we have 23456 % 9 = (2 + 3 + 4 + 5 + 6) % 9

   **几个重要的点：**

   1. 最终的结果res只有一位数字，那么res = res % 10 = (res == 9 ?  9 : res % 9)
   2. 若res != 9，num % 9 = res % 9 = res
   3. 若res == 9，res = 9

   **结论：**

   1. 当num % 9 != 0 或 num == 0，res = res % 9 = num % 9
   2. 当num % 9 == 0 且 num != 0，res = 9

   ```java
   public int addDigits(int num) {
       int temp = num % 9;
       return (temp == 0 && num != 0) ? 9 : temp; 
   }
   ```

   **进一步分析：**

   当n != 9 时：res = n % 9 = (n - 1) % 9 + 1

   当n == 9 时：res = 9 = (n - 1) % 9 + 1 != n % 9

   ```java
   public int addDigits(int num) {
       return 1 + (num - 1) % 9; 
   }
   ```

   

1. 数字根（digit root）公式。。。。。。。

   > For base *b* (decimal case *b* = 10), the digit root of an integer is:
   >
   > - dr(*n*) = 0 if *n* == 0
   > - dr(*n*) = (*b*-1) if *n* != 0 and *n* % (*b*-1) == 0
   > - dr(*n*) = *n* mod (*b*-1) if *n* % (*b*-1) != 0
   >
   > or
   >
   > - dr(*n*) = 1 + (*n* - 1) % 9
   >
   > Note here, when *n* = 0, since (*n* - 1) % 9 = -1, the return value is zero (correct).
   >
   > From the formula, we can find that the result of this problem is immanently periodic, with period (*b*-1).
   >
   > 
   >
   > Output sequence for decimals (*b* = 10):
   >
   > ~input: 0 1 2 3 4 ...
   > output: 0 1 2 3 4 5 6 7 8 9 1 2 3 4 5 6 7 8 9 1 2 3 ....

   
