#  39. Combination Sum

1. 回溯，空间O(Min(target，N))，时间emmm，等我做完Combination Sum 2之后再分析。

   ```java
   class Solution {
       private List<List<Integer>> res = new LinkedList<>();
       public List<List<Integer>> combinationSum(int[] candidates, int target) {
            Arrays.sort(candidates);
           findSolution(candidates, 0, target, new ArrayList<>());
           return this.res;
       }
       
       private void findSolution(int[] arr, int cur, int target, List<Integer> pre) {
           if(target < 0) {
               return;
           } else if(target == 0) {
               this.res.add(new ArrayList(pre));
           }
           
           for(int i = cur; i < arr.length; i++) {
               pre.add(arr[i]);
               findSolution(arr, i, target - arr[i], pre);
               pre.remove(pre.size() - 1);
           }
       }
   }
   ```

   