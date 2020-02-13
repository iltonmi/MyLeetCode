#  371. Sum of Two Integers

 https://leetcode.com/problems/sum-of-two-integers/discuss/84290/Java-simple-easy-understand-solution-with-explanation 

> For this, problem, for example, we have a = 1, b = 3,
>
> In bit representation, a = 0001, b = 0011,
>
> First, we can use "and"("&") operation between a and b to find a carry.
>
> carry = a & b, then carry = 0001

a & b 是相同的位，所以 (a & b) << 1是进位。

> Second, we can use "xor" ("^") operation between a and b to find the different bit, and assign it to a,
>
> Then, we shift carry one position left and assign it to b, b = 0010.

a ^ b 是不同的位，所以 a ^ b 不需要进位，但可能和上面提到的进位相加而产生进位。

>  Iterate until there is no carry (or b == 0) 

递归直到a & b（即不产生进位）为0，a ^ b就是结果。

1. 递归，空间时间O(1)。

   ```java
   public int getSum(int a, int b) {
   	return b == 0 ? a : getSum(a ^ b, (a & b) << 1);
   }
   ```

   

2. 迭代，空间时间O(1)。

   ```java
   public int getSum(int a, int b) {
           if(a == 0) {
               return b;
           }
           if(b == 0) {
               return a;
           }
           int carry = (a & b) << 1;
           int base = a ^ b;
           while(carry != 0) {
               int tempBase = base;
               base = base ^ carry;
               carry = (tempBase & carry) << 1;
           }
           return base;
       }
   ```

   