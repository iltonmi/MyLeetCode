#  75. Sort Colors 

 https://leetcode-cn.com/problems/sort-colors/solution/yan-se-fen-lei-by-leetcode/ 

1. 核心思想，用1个指针进行遍历，用2个指针将数组分为3个区域。

   （-1, left]上的值是0，(left, cur)上的值是1，[cur, right)上的值未检查，[right, arr.length)上的值是2.

   易错点：将nums[cur]的值和nums[right]的值进行交换后的下一个循环，检查nums[cur+1]的值。

   原因：nums[cur]是原来nums[right]的值，这个值未被遍历。

   ```java
   public void sortColors(int[] nums) {
       int left = -1;
       int right = nums.length;
       for(int cur = 0; cur < right; ) {
           if(nums[cur] < 1) {
               swap(nums, cur++, ++left);
           } else if(nums[cur] > 1) {
               //这种情况cur不自增
               swap(nums, cur, --right);
           } else {
               cur++;
           }
       }
   }
   
   private void swap(int[] arr, int a1, int a2) {
       if(a1 == a2) {
           return;
       }
       int temp = arr[a1];
       arr[a1] = arr[a2];
       arr[a2] = temp;
   }
   ```

   