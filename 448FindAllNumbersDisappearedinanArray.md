#  448. Find All Numbers Disappeared in an Array

1. 通过修改原数组的值，标记已经出现的数字，时间O(n)，空间O(1)，一般有2种做法：

   1. 标记为负，注意连续出现两次的数。

   2. 加n。

      以第二种做法为例，给出代码：

   ```java
   public List<Integer> findDisappearedNumbers(int[] nums) {
       List<Integer> res = new LinkedList<>()
       int n = nums.length;
       for(int i = 0; i < nums.length; ++i) {
           nums[(nums[i] - 1) % n] += nums[(nums[i] - 1) % n]; 
       }
       for(int i = 0; i < nums.length; ++i) {
           if(nums[i] <= n) {
               res.add(i + 1);
           }
       }
       return res;
   }
   ```

2. 使用hash table，标记已出现过的数字，时间O(n)，空间O(n)。虽然这种做法的时间复杂度和第1种一样，但其实仔细的看，复杂度的系数相差不少。

   ```java
   public List<Integer> findDisappearedNumbers(int[] nums) {
       int[] map = new int[nums.length+1];
       List<Integer> ans = new LinkedList();
       for (int i:nums){
           map[i]++;
       }
       for (int i=1; i<=nums.length; i++){
           map[i]--;
           if (map[i]==-1)
               ans.add(i);
       }
       return ans;
   }
   ```

   