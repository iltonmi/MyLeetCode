#  303. Range Sum Query - Immutable 



1. 暴力，空间O(N)

   ```java
   private int[] data;
   
   //时间O(1)
   public NumArray(int[] nums) {
       data = nums;
   }
   
   //时间O(N)
   public int sumRange(int i, int j) {
       int sum = 0;
       for (int k = i; k <= j; k++) {
           sum += data[k];
       }
       return sum;
   }
   ```

   

2. 暴力缓存，空间O(N ^ 2)

   ```java
   //Pair在javafx.util中。。。
   private Map<Pair<Integer, Integer>, Integer> map = new HashMap<>();
   
   //时间O(N ^ 2)
   public NumArray(int[] nums) {
       for (int i = 0; i < nums.length; i++) {
           int sum = 0;
           for (int j = i; j < nums.length; j++) {
               sum += nums[j];
               map.put(Pair.create(i, j), sum);
           }
       }
   }
   
   //时间O(1)
   public int sumRange(int i, int j) {
       return map.get(Pair.create(i, j));
   }
   ```

   

3. 一维缓存，空间O(N)

   ```java
   private int[] sum;
   
   //时间O(N)
   public NumArray(int[] nums) {
       sum = new int[nums.length + 1];
       for (int i = 0; i < nums.length; i++) {
           sum[i + 1] = sum[i] + nums[i];
       }
   }
   
   //时间O(1)
   public int sumRange(int i, int j) {
       return sum[j + 1] - sum[i];
   }
   ```

   