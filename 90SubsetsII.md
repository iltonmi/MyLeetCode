#  90. Subsets II 

1. 回溯，时间空间O(N * (2 ^ N))

   ```java
   class Solution {
       int n;
       int i;
       List<List<Integer>> res = new LinkedList<>();
       public List<List<Integer>> subsetsWithDup(int[] nums) {
           int n = nums.length;
           res.add(new ArrayList<>());
           Arrays.sort(nums);
           for(i = 1; i <= n; i++) {
               helper(0, new ArrayList<>(), nums);
           }
           return res;
       }
       
       private void helper(int start, List<Integer> tempList, int[] nums) {
           if(tempList.size() == i) {
               res.add(new ArrayList<>(tempList));
               return;
           }
           int tempSize = tempList.size();
           for(int j = start; j <= nums.length - i + tempSize; j++) {
               if(j > start && nums[j-1] == nums[j]){
                   continue;
               }
               tempList.add(nums[j]);
               helper(j + 1, tempList, nums);
               tempList.remove(tempList.size() - 1);
           }
       }
   }
   class Solution {
       List<List<Integer>> res = new LinkedList<>();
       public List<List<Integer>> subsetsWithDup(int[] nums) {
           Arrays.sort(nums);
           helper(0, new ArrayList<>(), nums);
           return res;
       }
       
       private void helper(int start, List<Integer> tempList, int[] nums) {
           res.add(new ArrayList<>(tempList));
           int tempSize = tempList.size();
           for(int j = start; j < nums.length; j++) {
               if(j > start && nums[j-1] == nums[j]) {
                   continue;
               }
               tempList.add(nums[j]);
               helper(j + 1, tempList, nums);
               tempList.remove(tempList.size() - 1);
           }
       }
   }
   ```

   

2. 迭代，时间空间O(N * (2 ^ N))

   ```java
   public List<List<Integer>> subsetsWithDup(int[] nums) {
       Arrays.sort(nums);
       List<List<Integer>> res = new ArrayList<>();
       res.add(new ArrayList<>());
       int oldLen = 0;
       for(int i = 0; i < nums.length; i++) {
           int startIndex = (i >= 1 && nums[i] == nums[i - 1]) ? oldLen : 0;
           oldLen = res.size(); 
           for(int j = startIndex; j < oldLen; j++){
               List<Integer> temp = res.get(j);
               List<Integer> subset = new ArrayList<>(temp);
               subset.add(nums[i]);
               res.add(subset);
           }
       }
       return res;
   }
   ```

   