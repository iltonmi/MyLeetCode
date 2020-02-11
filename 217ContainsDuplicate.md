#  217. Contains Duplicate

索然无味。

 https://leetcode.com/articles/contains-duplicate/ 

1. 暴力（超时），空间O(1)，时间O(N ^ 2)。

   ```java
   public boolean containsDuplicate(int[] nums) {
       for (int i = 0; i < nums.length; ++i) {
           for (int j = 0; j < i; ++j) {
               if (nums[j] == nums[i]) {
                   return true;  
               }
           }
       }
       return false;
   }
   ```

   

2. 排序，空间时间O(取决于排序算法)。

   ```java
   public boolean containsDuplicate(int[] nums) {
       Arrays.sort(nums);
       for (int i = 0; i < nums.length - 1; ++i) {
           if (nums[i] == nums[i + 1]) return true;
       }
       return false;
   }
   ```

   

3. 散列表，空间O(N)，时间O(N)。

   ```java
   public boolean containsDuplicate(int[] nums) {
       Set<Integer> set = new HashSet<>(nums.length);
       for (int x: nums) {
           if (set.contains(x)) return true;
           set.add(x);
       }
       return false;
   }
   ```

   