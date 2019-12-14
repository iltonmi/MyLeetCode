#  46. Permutations

复杂度下回分解。

1. HashSet，暴力枚举

   ```java
   class Solution {
       private List<List<Integer>> res = new LinkedList<>();
       private HashSet<Integer> set = new HashSet<>();
       public List<List<Integer>> permute(int[] nums) {
           help(nums, new ArrayList<>());
            return res;
       }
       
       private void help(int[] arr, List<Integer> pre) {
           if(pre.size() == arr.length) {
               res.add(new ArrayList<>(pre));
               return;
           }
           for(int i = 0; i < arr.length; i++) {
               if(set.contains(arr[i])) {
                   continue;
               }
               pre.add(arr[i]);
               set.add(arr[i]);
               help(arr, pre);
               pre.remove(pre.size() - 1);
               set.remove(arr[i]);
           }
       }
   }
   ```

   

2. 回溯

   ```java
   class Solution {
       List<List<Integer>> res = new LinkedList();
       public List<List<Integer>> permute(int[] nums) {
           // convert nums into list since the output is a list of lists
           ArrayList<Integer> numList = new ArrayList<Integer>();
           for (int num : nums)
             numList.add(num);
           backtrack(numList, 0);
           return res;
       }
       
       private void backtrack(List<Integer> numList, int first) {
           if(first == numList.size()) {
               res.add(new ArrayList<Integer>(numList));
           }
           
           for(int i = first; i < numList.size(); i++) {
               Collections.swap(numList, first, i);
               backtrack(numList, first + 1);
               Collections.swap(numList, first, i);
           }
       }
   }
   ```

   