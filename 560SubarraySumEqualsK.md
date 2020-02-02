#  560. Subarray Sum Equals K

 https://leetcode.com/articles/subarray-sum-equals-k/ 

1. 常规做法，空间O(1)，时间O(N ^ 2)

   ```java
   public class Solution {
       public int subarraySum(int[] nums, int k) {
           int count = 0;
           for (int start = 0; start < nums.length; start++) {
               int sum=0;
               for (int end = start; end < nums.length; end++) {
                   sum+=nums[end];
                   if (sum == k)
                       count++;
               }
           }
           return count;
       }
   }
   ```

   

2. 散列表，空间O(N)，时间O(N)

   1. 遍历累计和sum，若sum第一次出现，放入散列表；否则将其出现次数加1。
   2. 每次遇到sum，查询 sum - k 出现的次数。sum - k的出现，意味着累计和为sum - k的索引 i 和当前所有 j 之间的值总和为k。
   
   ```java
   class Solution {
       private Map<Integer, Integer> map = new HashMap<>();
       public int subarraySum(int[] nums, int k) {
           int res = 0;
           int sum = 0;
           //哨兵值
           map.put(0, 1);
           for(int num : nums) {
               sum += num;
               if(map.containsKey(sum - k)) {
                   res += map.get(sum - k);
               }
               map.put(sum, map.getOrDefault(sum, 0) + 1);
           }
           return res;
    }
   }
   ```
   
   