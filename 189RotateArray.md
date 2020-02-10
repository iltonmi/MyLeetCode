#  189. Rotate Array

 https://leetcode.com/articles/rotate-array/ 

 https://leetcode-cn.com/problems/rotate-array/solution/xuan-zhuan-shu-zu-by-leetcode/ 



1. 暴力旋转k次，每次旋转1位，空间O(1)，时间O(N * k)。

2. 使用额外的数组，空间O(N)，时间O(N)。

3. 环状替换，空间O(1)，时间O(N)。

    ![image.png](https://pic.leetcode-cn.com/f0493a97cdb7bc46b37306ca14e555451496f9f9c21effcad8517a81a26f30d6-image.png) 

   ```java
   public void rotate(int[] nums, int k) {
       k %= nums.length;
       //count 移动的次数
       int count = 0;
       //移动次数等于N则退出循环
       for (int start = 0; count < nums.length; start++) {
           int prev = nums[start];
           int cur = (start + k) % nums.length;
           //避免cur == start会进入死循环
           for(; cur != start; cur = (cur + k) % nums.length) {
               int temp = nums[cur];
               nums[cur] = prev;
               prev = temp;
               count++;
           }
           //初始位置的新值 = 最后被填补的位置的值
           nums[cur] = prev;
           count++;
       }
   }
   ```

   

4. 使用反转（目瞪口呆），空间O(1)，时间O(N)。特型算法。

   > 这个方法基于这个事实：当我们旋转数组 k 次， k%n 个尾部元素会被移动到头部，剩下的元素会被向后移动。
   >
   > 在这个方法中，我们首先将所有元素反转。然后反转前 k 个元素，再反转后面 n−k 个元素，就能得到想要的结果。
   >
   > 假设 n=7 且 k=3 。
   >
   > 原始数组                  : 1 2 3 4 5 6 7
   > 反转所有数字后             : 7 6 5 4 3 2 1
   > 反转前 k 个数字后          : 5 6 7 4 3 2 1
   > 反转后 n-k 个数字后        : 5 6 7 1 2 3 4 --> 结果

   ```java
   public void rotate(int[] nums, int k) {
       k %= nums.length;
       reverse(nums, 0, nums.length - 1);
       reverse(nums, 0, k - 1);
       reverse(nums, k, nums.length - 1);
   }
   public void reverse(int[] nums, int start, int end) {
       while (start < end) {
           int temp = nums[start];
           nums[start] = nums[end];
           nums[end] = temp;
           start++;
           end--;
       }
   }
   ```

   