# 260. Single Number III

1.  集合，一次遍历，空间复杂度O(N)，时间复杂度O(N)

   ```java
   public int[] singleNumber(int[] nums) {
       int[] res = new int[2];
       if(nums == null || nums.length < 2) {
           return res;
       }
       Set<Integer> set = new HashSet<>(nums.length);
       for(int i = 0; i < nums.length; i++) {
           int cur = nums[i];
           if(set.contains(cur)) {
               set.remove(cur);
           } else {
               set.add(cur);
           }
       }
       boolean first = true;
       for(Integer ele : set) {
           if(first) {
               res[0] = ele;
               first = !first;
           } else {
               res[1] = ele;
           }
       }
       return res;
   }
   ```

   

2. 位运算，得到只出现1的两个数的二进制形式中，最右边的不一样的地方。空间复杂度O(1)，时间复杂度O(N)

   ```java
   public int[] singleNumber(int[] nums) {
       if(nums == null || nums.length < 2) {
           return new int[2];
       }
       int diff = 0;
       for(int i : nums) {
           diff ^= i;
       }
       //获取二进制形式中，答案的2个数的最右边不一样的地方
       int rightMostDiff = diff & (-diff);
       int val = 0;
       for(int i : nums) {
           if((i & rightMostDiff) == 0) {
               //只有出现两次的数，和其中一个出现一次的数会进入这里
               val ^= i;
           }
       }
       return new int[]{val, val ^ diff};
   }
   ```

   