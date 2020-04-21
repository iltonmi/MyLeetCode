 # 78. Subsets

1. 迭代，空间时间O(N * (2^N))

   ```java
   public List<List<Integer>> subsets(int[] nums) {
       List<List<Integer>> res = new ArrayList<>();
       res.add(new ArrayList<>());
       for(int n : nums){
           int oldLen = res.size();
           for(int i = 0; i < oldLen; i++){
               List<Integer> subset = new ArrayList<>(res.get(i));
               subset.add(n);
               res.add(subset);
           }
       }
       return res;
   }
   ```

   

2. 回溯（通用解法），空间时间O(N * (2^N))

   ```java
   class Solution {
       private List<List<Integer>> res;
       public List<List<Integer>> subsets(int[] nums) {
           this.res = new ArrayList<>((int)Math.pow(2, nums.length));
           backtrack(0, new ArrayList<>(), nums);
           return res;
       }
   
       private void backtrack(int start, List<Integer> tempList, int [] nums){
           res.add(new ArrayList<>(tempList));
           for(int i = start; i < nums.length; i++){
               tempList.add(nums[i]);
               backtrack(i + 1, tempList, nums);
               tempList.remove(tempList.size() - 1);
           }
       }
   }
   ```

   

3. 骚操作，空间时间O(N * (2^N))，将nums数组中的数字是否应该存在于某个subset中，映射到int的二进制表达中。

   例如，集合[1,2,3]中：整数0，即000，对应的自己为 [ ]；而整数2，即010，对应的子集为 [2]；而整数5，即101，对应的子集为 [1, 3]。

   ```java
   public List<List<Integer>> subsets(int[] nums) {
       int total = 1 << nums.length;
       List<List<Integer>> res = new ArrayList<>(total);
       //00..00到11..11, n位数
       for (int i = 0; i < total; i++) {
           List<Integer> subset = new LinkedList<>();
           for (int j = 0; j < nums.length; j++) {
               //第j位上不是0，则添加
               if ((i & (1 << j)) != 0) {
                   subset.add(nums[j]);
               }
           }
           res.add(subset);
       }
       return res;
   }
   ```

   