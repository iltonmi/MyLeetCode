# 259 3Sum Smaller

对于3sum的问题，其实就是指定一个值a之后，去解决2sum的问题。

 https://leetcode-cn.com/problems/3sum-smaller/solution/jiao-xiao-de-san-shu-zhi-he-by-leetcode/ 

1. 二分查找，外部枚举O(N)，内部枚举O(N)，二分查找O(logN)，空间复杂度O(1)，时间复杂度O(N ^ 2 * logN)

   ```java
   public int threeSumSmaller(int[] nums, int target) {
       Arrays.sort(nums);
       int sum = 0;
       for(int i = 0; i < nums.length - 2; i++) {
           sum += twoSumSmaller(nums, i + 1, target - nums[i]);
       }
       return sum;
   }
   
   private int twoSumSmaller(int[] arr, int start, int target) {
       int res = 0;
       for(int i = start; i < arr.length - 1; i++) {
           //这里包含i,因为j - i可以是0
           int j = binarySearch(arr, i, target - arr[i]);
           res += (j - i);
       }
       return res;
   }
   
   //查找比目标值小的最大值
   private int binarySearch(int[] arr, int start, int target) {
       int l = start;
       int r = arr.length - 1;
       while(l < r) {
           int mid = r + (l - r) / 2;
           if(arr[mid] < target) {
               l = mid;
           } else {
               r = mid - 1;
           }
       }
       return l;
   }
   ```

   

2. 双指针，空间复杂度O(1)，时间复杂度O(N ^ 2)

   ```java
   public int threeSumSmaller(int[] nums, int target) {
       Arrays.sort(nums);
       int sum = 0;
       for(int i = 0; i < nums.length - 2; i++) {
           sum += twoSumSmaller(nums, i + 1, target - nums[i]);
       }
       return sum;
   }
   
   private int twoSumSmaller(int[] arr, int start, int target) {
       int l = start;
       int r = arr.length - 1;
       int res = 0;
       while(l < r) {
           if(arr[l] + arr[r] < target) {
               res += r - l;
               l++;
           } else {
               r--;
           }
       }
       return res;
   }
   ```

   