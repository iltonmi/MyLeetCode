# 55. Jump Game

1. 正推, 空间O(n), 时间O(n^2)

   ```java
   public boolean canJump(int[] nums) {
       boolean[] arrived = new boolean[nums.length];
       arrived[0] = true;
       for(int i = 0; i < nums.length - 1; i++) {
           if(arrived[i]) {
               for(int j = i; j < nums.length && j <= i + nums[i]; j++) {
                   arrived[j] = true;
               }
           }
       }
       return arrived[nums.length - 1];
   }
   ```

2. 反推

   一个很简单的想法：从最后一个元素开始逐个向前遍历。

   最初的想法：

   1. 在遍历过程中，看哪个nums[i]能达到最后一个元素，然后再看哪个元素能达到nums[i]。

   2. 以此类推，最后就能知道能够从第一个位置到达最后一个位置。
   3. 因此，需要一个变量记录需要到达的位置，记为lastPos。

   提出问题：

   1. 假如，有多个值能到达lastPos：在当次遍历，只能选择其中一个值，可能被选择的值不能从初始位置到达，而没被选择的值中包含能从初始位置到达，从而出现漏解的情况呢？

      **解答：**

      1. 假如存在多个值能到达lastPos，按照从后开始逐个向前遍历，被选择的值必然是最右值。
      2. 既然最右值以外的值能到达lastPos，就必然能到达最右值。那么，最左值也能到达最右值。
      3. 因此，若有多个值能到达lastPos，那lastPos在遍历中必然会被赋值为最左值。
      4. 宏观角度，若有多个值能到达lastPos，总是选择最左的值。
      5. 那么现在**问题变成了：**假如，有多个值能到达lastPos：如果总是选择最左的值，有没有可能：最左的值不能从初始位置到达，但除此之外在右边的值能从初始位置到达呢？
      6. **解答：**不可能，假设初始位置能够到达位置k，然后k能到达除最左值外的任意可能值，那么k也必然能到达最左值。

   2. 占位

   **总结：**

   1. 因为每次都选择最左边能到达lastPos的值，因此这其实是个贪心算法。
   2. 贪心算法的难度在于贪心策略的证明，经过**问题1**的证明，可以知道这样不会出现漏解的情况。

   ```java
   public boolean canJump(int[] nums) {
       int lastPos = nums.length - 1;
       for(int i = lastPos; i >= 0; i--) {
           if(i + nums[i] >= lastPos) {
               lastPos = i;
           }
       }
       return lastPos == 0;
   }
   ```

   

3. 占位