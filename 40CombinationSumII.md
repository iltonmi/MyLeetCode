#  40. Combination Sum II 

1. 回溯，排序剪枝，空间O(N)，时间O(N ^ 2)

   ```java
   class Solution {
       private int[] arr;
       private List<Integer> temp = new ArrayList<>();
       private List<List<Integer>> res = new LinkedList<>();
       
       public List<List<Integer>> combinationSum2(int[] candidates, int target) {
           if(candidates == null || candidates.length == 0 || target < 0) {
               return res;
           }
           Arrays.sort(candidates);
           this.arr = candidates;
           help(0, target);
           return res;
       }
       
       private void help(int start, int target) {
           if(start == arr.length) {
               return;
           }
           int pre = -1;
           for(int i = start; i < arr.length; i++) {
               int cur = arr[i];
               if(cur == pre) {
                   continue;
               }
               if(cur < target) {
                   temp.add(cur);
                   help(i + 1, target - cur);
                   temp.remove(temp.size() - 1);
                   pre = cur;
               } else {
                   if(cur == target) {
                       temp.add(cur);
                       res.add(new ArrayList(temp));
                       temp.remove(temp.size() - 1);
                   }
                   return;
               }
           }
       }
   }
   ```

   

2. 