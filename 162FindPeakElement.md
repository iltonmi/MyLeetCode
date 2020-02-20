# 162. Find Peak Element

二分找到的元素其实只是局部peak element，但题目要求返回任意peak element即可。

1. 一次遍历，空间O(1)，时间O(N)

   因为题目规定 nums [-1] = nums [n] = MIN_VALUE

   因此容易保证 nums [i] > nums [i-1]，一旦 nums [i] > nums [i+1] 就说明 nums [i] 是 peak element

   ```java
   public int findPeakElement(int[] nums) {
       for(int i = 0; i < nums.length - 1; i++) {
           if(nums[i] > nums[i+1]) {
               return i;
           }
       }
       return nums.length - 1;
   }
   ```

   

2. 递归二分，空间O(logN)，时间O(logN)

   ```java
   public int findPeakElement(int[] nums) {
       return search(nums, 0, nums.length - 1);
   }
   
   public int search(int[] nums, int l, int r) {
       if (l == r) {
           return l;
       }
       int mid = (l + r) / 2;
       return nums[mid] > nums[mid + 1] 
           ? search(nums, l, mid) : search(nums, mid + 1, r);
   }
   ```

   

3. 迭代二分 ，空间O(1)，时间O(logN)

   ```java
   public int findPeakElement(int[] nums) {
       int l = 0, r = nums.length - 1;
       while (l < r) {
           int mid = (l + r) / 2;
           if (nums[mid] > nums[mid + 1]) {
               //注意这里不是 mid - 1, 不然会漏解
               r = mid;
           } else {
               l = mid + 1;   
           }
       }
       return l;
   }
   ```

   