#  169. Majority Element

1. 摩尔投票法，空间O(1)，时间O(N)

   **世界大战** ：

   1. **战争规则**是1对1拼刺刀，所有战士的水平都不相上下，拼刺刀的结果总是2名参与者同时阵亡；士兵不能自杀；众数，即mj国的兵力大于 `⌊ n/2 ⌋`。 
   2. **战争场地数量N**+1个。参战人数2K+1个，即mj国至少有K+1个战士。
   3. **前N个战场的战况**：所有士兵互相抵消，无一幸存，总参战人数2X。
   4. **前N个战场的分析**：因为士兵不能自杀，所有mj国派兵数量最多是X。
   5. **最后1个战场的分析**：必定是mj国胜利。因为战争开始前，mj国士兵数量大于其他国的总和，而且mj国在前N个战场派兵最多也是其他国家派兵的总和，所以在最后一个战场士兵数量依然大于其他国的总和。（**我比你多钱，我俩花出去一样的钱，剩下的钱我依然比你多**）

   ```java
   public int majorityElement(int[] nums) {
       //当前区域的众数
       int res = 0;
       //当前区域未被抵消的众数数量
       int count = 0;
       for(int num : nums) {
           if(count == 0) {
               res = num;
           }
           count += (num == res ? 1 : -1);
       }
       return res;
   }
   ```

   

2. 散列表记录出现次数，空间O(N)，时间O(N)

3. 排序，返回中间位置的元素，空间时间取决于排序算法。

4. DAC，空间O(logN)，时间O(NlogN)

   ```java
   class Solution {
       private int countInRange(int[] nums, int num, int lo, int hi) {
           int count = 0;
           for (int i = lo; i <= hi; i++) {
               if (nums[i] == num) {
                   count++;
               }
           }
           return count;
       }
   
       private int majorityElementRec(int[] nums, int lo, int hi) {
           // base case; the only element in an array of size 1 is the majority
           // element.
           if (lo == hi) {
               return nums[lo];
           }
   
           // recurse on left and right halves of this slice.
           int mid = (hi-lo)/2 + lo;
           int left = majorityElementRec(nums, lo, mid);
           int right = majorityElementRec(nums, mid+1, hi);
   
           // if the two halves agree on the majority element, return it.
           if (left == right) {
               return left;
           }
   
           // otherwise, count each element and return the "winner".
           int leftCount = countInRange(nums, left, lo, hi);
           int rightCount = countInRange(nums, right, lo, hi);
   
           return leftCount > rightCount ? left : right;
       }
   
       public int majorityElement(int[] nums) {
           return majorityElementRec(nums, 0, nums.length-1);
       }
   }
   ```

   