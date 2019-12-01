#  283. Move Zeroes

1. 迭代，时间O(n)，时间O(1)

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

2. 修改原数据，将所有非0复制到前面，最后将后面赋值为0

   ```java
   public void moveZeroes(int[] nums) {
       if(nums == null) {
           return;
       }
       int mark = 0;
       for(int i = 0; i < nums.length; i++) {
           if(nums[i] != 0) {
               nums[mark] = nums[i];
               mark++;
           }
       }
       for(; mark < nums.length; mark++) {
           nums[mark] = 0;
       }
   }
   ```

   