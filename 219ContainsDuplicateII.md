#  219. Contains Duplicate II 

1. 滑动窗口

   ```java
   public boolean containsNearbyDuplicate(int[] nums, int k) {
      Set<Integer> set = new HashSet<>();
       for (int i = 0; i < nums.length; ++i) {
           if (set.contains(nums[i])) {
               return true;
           }
           set.add(nums[i]);
           if (set.size() > k) {
               set.remove(nums[i - k]);
           }
       }
       return false;
   }
   ```

2. hashmap，不需要维持滑动窗口，只需要保存某个数字最近出现的位置。

   因为，某个数字新出现的位置会比之前出现的位置更容易满足题目条件:

   ​	（nums[i] == nums[j], abs( i - j ) <= k）

   ```java
   public boolean containsNearbyDuplicate(int[] nums, int k) {
       Map<Integer, Integer> map = new HashMap<>(nums.length);
       for (int i = 0; i < nums.length; ++i) {
           int cur = nums[i];
           Integer pos = map.get(cur);
           if(pos != null && i - pos <= k) {
               return true;
           }
           map.put(cur, i);
       }
       return false;
   }
   ```

   