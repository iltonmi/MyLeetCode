#  88. Merge Sorted Array

1. 将nums2复制得到nums1的尾部，时间负责度O((N+M)(log(N+M)))。还可以更好，这种做法没有利用数组的有序性。

2. 一般的想法，使用一个长度为m的数组储存nums1的元素，比较nums1和nums2的每个元素，将更小的复制到输出数组，空间复杂度O(M)，时间复杂度O(N+M)。

   ```java
   public void merge(int[] nums1, int m, int[] nums2, int n) {
       int[] nums = new int[m];
       System.arraycopy(nums1, 0, nums, 0, m);
       int p = 0;
       int p1 = 0;
       int p2 = 0;
       while(p1 < m && p2 < n) {
           nums1[p++] = nums[p1] < nums2[p2] ? nums[p1++] : nums2[p2++];
       }
       if(p2 < n) {
           System.arraycopy(nums2, p2, nums1, p, n - p2);
       } else if(p1 < m) {
           System.arraycopy(nums, p1, nums1, p, m - p1);
       }
   }
   ```

   

3. 特别的想法，从后往前比较nums1和nums2的元素，将更大的复制到nums1的适当位置，空间复杂度O(1)，时间复杂度O(N+M)。

   ```java
   public void merge(int[] nums1, int m, int[] nums2, int n) {
       int p = nums1.length - 1;
       int p1 = m - 1;
       int p2 = n - 1;
       while(p1 >= 0 && p2 >= 0) {
           nums1[p--] = nums1[p1] > nums2[p2] ? nums1[p1--] : nums2[p2--];
       }
       if(p2 >= 0) {
           System.arraycopy(nums2, 0, nums1, 0, p2 + 1);
       }
   }
   ```

   