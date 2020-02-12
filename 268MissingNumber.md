#  268. Missing Number

1. 排序，空间时间取决于排序算法。

2. 散列，空间O(N)，时间O(N)。

3. （反例）高斯求和（溢出），空间O(1)，时间O(N)。

   1. 先求和，再减，明显溢出。

      ```java
      //溢出的反例
      public int missingNumber(int[] nums) {
          int expectedSum = nums.length * (nums.length + 1) / 2;
          int actualSum = 0;
          for (int num : nums) {
              actualSum += num;
          }
          return expectedSum - actualSum;
      }
      ```

      

   2. （容易忽略）加减同时进行，也会溢出：

      用下面这段代码举例：

      ```java
      避免不了风险，例如：
          第一次循环： sum = 1 - Integer.MAX_VALUE；
          第二次循环：sum += 2 - (Integer.MAX_VALUE - 1)；
          2次循环结束后，sum = 3 - 2 * Integer.MAX_VALUE，sum溢出了。
      ```

      ```java
      溢出的情况，通俗的来说：前两次循环减了最大的2个整型值
      ```

      ```java
      //溢出的反例
      public int missingNumber(int[] nums) {
          int sum = 0;
          for (int i = 1; i <= nums.length; i++) {
              //加减同时进行，依然溢出。
              sum += i;；
              sum -= nums[i - 1];
          }
          return sum;
      }
      ```

      

   

4. 位运算，空间O(1)，时间O(N)。

   很巧妙，利用了**位运算的技巧，N ^ N = 0, N ^ 2 = N，且异或满足结合律**。

   因此，将0 ... N 异或一次，再将数组的所有数遍历一次，2次出现的数就是没有缺失的数，它们会变成0。

   最后的结果就是缺失的数，因为其他数字异或的结果是0，而 0 ^ res = res。

   ```java
   public int missingNumber(int[] nums) {
       //0 ^ n
       int res = nums.length;
       for (int i = 0; i < nums.length; i++) {
           res ^= i ^ nums[i];
       }
       return res;
   }
   ```

   