#  283. Move Zeroes

1. 迭代，时间O(N)，时间O(1)，好青涩的代码，不愧是刚开始的我。

   ```java
   public void moveZeroes(int[] nums) {
      if(nums == null || nums.length == 1) {
           return;
       }
       //
       int low = 0;
       int high = 0;
       while(high < nums.length) {
           //low必须小于high
           if(low == high || nums[high] == 0) {
               //high往前，寻找可被替换的非0数
               high++;
           } else {
               if(nums[low] != 0) {
                   //low往前，寻找需要替换的0
                   low++;
               } else {
                   int temp = nums[low];
                   nums[low] = nums[high];
                   nums[high] = temp;
                   low++;
                   high++;
               }
           }
       }
   }
   ```

2. 修改原数据，将所有非0复制到前面，最后将后面赋值为0

   ```java
   public void moveZeroes(int[] nums) {
       if(nums == null) {
           return;
       }
       int mark = 0;
       for(int i = 0; i < nums.length; i++) {
           if(nums[i] != 0) {
               nums[mark++] = nums[i];
           }
       }
       for(; mark < nums.length; mark++) {
           nums[mark] = 0;
       }
   }
   ```
   
3. 优化。

      > 如果当前元素是非 0 的，那么它的正确位置最多可以是当前位置或者更早的位置。如果是后者，则当前位置最终将被非 0 或 0 占据，该非 0 或 0 位于大于 “cur” 索引的索引处。我们马上用 0 填充当前位置，这样不像以前的解决方案，我们不需要在下一个迭代中回到这里。
      >
      > https://leetcode-cn.com/problems/move-zeroes/solution/yi-dong-ling-by-leetcode/ 

   ```java
   public void moveZeroes(int[] nums) {
       for (int lastNonZeroFoundAt = 0, cur = 0; cur < nums.length; cur++) {
           if (nums[cur] != 0) {
               int temp = nums[cur];
               nums[cur] = nums[lastNonZeroFoundAt];
               nums[lastNonZeroFoundAt] = temp;
               lastNonZeroFoundAt++;
           }
       }
   }
   ```

   



   