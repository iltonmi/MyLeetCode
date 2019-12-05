#  11. Container With Most Water 

1. 双指针，注重策略证明，时间O(n), 空间O(1)。

   证明前，需要明确，蓄水池容量的上限就是两块木板中的短板。
   
   **基本条件：**
   
   1. 首先，对于height[0]和height[length - 1]，其中的短板决定了当前蓄水池的容积。
   2. 底部长度而言：你不能找到另一个蓄水池的底部比当前蓄水池的底部的长度值更大。
   3. 深度而言：这个短板就是它自己可能组成的所有蓄水池的深度上限。
   4. 总结：对于当前短板，它可能组成的蓄水池的容积已经达到了最大。
   5. 换言之，对于求最大容积这个问题，在后面考虑可能的蓄水池时，考虑当前蓄水池的短板已经没有意义。
   
   **后续过程：**
   
   1. 总是忽略前面确定已经没有考虑价值的“短板们”，在剩余的值中找到底部长度值最大的两条边界。
   2. 对于其中的短板，总能确定包含这个短板的剩余的可能性的最大容积（此短板的另一部分可能性已经在前面被间接证明对最终结果没有影响）。
   3. 若两条边界高度相等，即不存在的短板的情况，相当于同时能确定两条边界的剩余的可能性的最大容积。因此，将其中任意一个归为短板，甚至同时把两个都归为短板（亲测有效，但是多了分支慢了1ms），都不影响最终结果正确性。
   
   **总结：**每次对边界的考虑，都能找到一部分可能性中的最大值，从而排除掉这部分可能性，加速后续过程。
   
   
   ```java
   public int maxArea(int[] height) {
       int res = 0;
   
       int left = 0;
       int right = height.length - 1;
       while(left < right) {
           res = Math.max(res,
                          (right - left) * Math.min(height[left], height[right]));
           if(height[left] < height[right]) {
               left++;
           } else {
               right--;
           }
       }
       return res;
   }
   ```
   
2. 暴力，时间O(n^2), 空间O(1)

   ```java
   public int maxArea(int[] height) {
       int res = 0;
   
       for(int i = 0; i < height.length; i++) {
           for(int j = 0; j < height.length; j++) {
               res = Math.max(res, (j - i) * Math.min(height[i], height[j]));
           }
       }
       return res;
   }
   ```