#  47. Permutations II 

1. 回溯，空间O(N)，时间O(N!)，和46. Permutations的区别仅仅是

   ```java
   class Solution {
       List<List<Integer>> res = new LinkedList();
       public List<List<Integer>> permuteUnique(int[] nums) {
           // convert nums into list since the output is a list of lists
           ArrayList<Integer> numList = new ArrayList<Integer>();
           for (int num : nums) {
               numList.add(num);
           }
           backtrack(numList, 0);
           return res;
       }
       
       private void backtrack(List<Integer> numList, int first) {
           if(first == numList.size()) {
               res.add(new ArrayList<Integer>(numList));
           }
           Set<Integer> appeared = new HashSet<>();
           for(int i = first; i < numList.size(); i++) {
               if(appeared.add(numList.get(i))) {
                   Collections.swap(numList, first, i);
                   backtrack(numList, first + 1);
                   Collections.swap(numList, first, i);
               }
           }
       }
   }
   class Solution {
       List<List<Integer>> res = new LinkedList();
       public List<List<Integer>> permuteUnique(int[] nums) {
           // convert nums into list since the output is a list of lists
           backtrack(nums, 0);
           return res;
       }
       
       private void backtrack(int[] nums, int first) {
           if(first == nums.length) {
               List<Integer> temp = new ArrayList<>(nums.length);
               Arrays.stream(nums).forEach(val -> temp.add(val));
               res.add(temp);
           }
           Set<Integer> appeared = new HashSet<>();
           for(int i = first; i < nums.length; i++) {
               if(appeared.add(nums[i])) {
                   swap(nums, first, i);
                   backtrack(nums, first + 1);
                   swap(nums, first, i); 
               }
           }
       }
       
       private void swap(int[] arr, int a, int b) {
           int temp = arr[a];
           arr[a] = arr[b];
           arr[b] = temp;
       }
   }
   ```

2. 正经儿枚举

   ```java
   public class Solution {
       List<List<Integer>> res = new ArrayList<List<Integer>>();
       boolean[] visited;
       
       public List<List<Integer>> permuteUnique(int[] nums) {
           Arrays.sort(nums);
           List<Integer> cur = new ArrayList<Integer>();
           visited = new boolean[nums.length];
           permute(cur, nums);
           return res;
       }
       
       private void permute(List<Integer> cur, int[] nums) {
           if (cur.size() == nums.length) {
               res.add(new ArrayList<Integer>(cur));
               return;
           }
           for (int i = 0; i < visited.length; i++) {
               if (!visited[i]) {
                   if (i > 0 && nums[i] == nums[i-1] && visited[i-1]) {
                       return;
                   }
                   visited[i] = true;
                   cur.add(nums[i]);
                   permute(cur, nums);
                   cur.remove(cur.size() - 1);
                   visited[i] = false;
               }
           }
       }
   }
   ```

   