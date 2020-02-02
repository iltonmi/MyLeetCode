#  494. Target Sum

第1，2种自己能想出来，第3种背包分类的思想很重要。

1. 暴力递归（竟然没有超时），空间O(N), 时间O(2 ^ N)

   ```java
   class Solution {
       public int findTargetSumWays(int[] nums, int S) {
           return help(nums, S, 0);
       }
       
       private int help(int[] nums, int res, int cur) {
           if(cur == nums.length) {
               return res == 0 ? 1 : 0;
           }
           
           return help(nums, res - nums[cur], cur + 1) 
               + help(nums, res + nums[cur], cur + 1);
       }
   }
   ```

   

2. DP，自底向上

   dp [i] [j] = num[0 .. i] 有多少种组合的和为 j。j 的范围是 [-sum, sum]。

   1. dp [i] [j] = dp [i-1] [j-nums [i]] + dp [i-1] [j+nums [i]]
   2. 通俗来说，dp [i] [j] 依赖它上方的2个位置的元素。
   2. 基本条件：dp[0] [j] = nums [0] == j ? 1 : 0

   **S可能是负数，映射把它变成非负数，因为结果是对称的。**

   ```java
   class Solution {
       public int findTargetSumWays(int[] nums, int S) {
           if(nums == null || nums.length == 0) {
               return 0;
           }
           int sum = 0;
           for(int num : nums) {
               sum += num;
           }
           if(S < -sum || S > sum) {
               return 0;
           }
           int[][] dp = new int[nums.length][2 * sum + 1];
           for(int j = 0; j < dp[0].length; j++) {
               dp[0][j] = (nums[0] == j - sum ? 1 : 0) 
                   + (nums[0] == sum - j ? 1 : 0);
           }
           for(int i = 1; i < nums.length; i++) {
               for(int j = 0; j < dp[0].length; j++) {
                   dp[i][j] = (j-nums[i] >= 0 ? dp[i-1][j-nums[i]] : 0) 
                       + (j+nums[i] < dp[0].length ? dp[i-1][j+nums[i]] : 0);
               }
           }
           return dp[nums.length-1][S+sum];
       }
   }         
   ```

   压缩版，无法压缩。

   

3. **背包问题思路**。和第416题一样额。 

    https://leetcode.com/problems/target-sum/discuss/97334/Java-(15-ms)-C%2B%2B-(3-ms)-O(ns)-iterative-DP-solution-using-subset-sum-with-explanation 

   

> The original problem statement is equivalent to:
> Find a **subset** of `nums` that need to be positive, and the rest of them negative, such that the sum is equal to `target`
>
> Let `P` be the positive subset and `N` be the negative subset
>
> For example:
> Given `nums = [1, 2, 3, 4, 5]` and `target = 3` then one possible solution is `+1-2+3-4+5 = 3`
> Here positive subset is `P = [1, 3, 5]` and negative subset is `N = [2, 4]`
>
> Then let's see how this can be converted to a subset sum problem:
>
> ```java
>                   sum(P) - sum(N) = target
> sum(P) + sum(N) + sum(P) - sum(N) = target + sum(P) + sum(N)
>                        2 * sum(P) = target + sum(nums)
> ```
>
> So the original problem has been converted to a subset sum problem as follows:
> Find a **subset** `P` of `nums` such that `sum(P) = (target + sum(nums)) / 2`
>
> 
>
> Note that the above formula has proved that `target + sum(nums)` must be **even**
> We can use that fact to quickly identify inputs that do not have a solution (Thanks to @BrunoDeNadaiSarnaglia for the suggestion)
> For detailed explanation on how to solve subset sum problem, you may refer to [Partition Equal Subset Sum](https://leetcode.com/problems/partition-equal-subset-sum/)
>
> Here is Java solution (15 ms)
>
> ```java
> public int findTargetSumWays(int[] nums, int s) {
>     int sum = 0;
>     for (int n : nums)
>         sum += n;
>     return sum < s || (s + sum) % 2 > 0 ? 0 : subsetSum(nums, (s + sum) >>> 1); 
> }   
> 
> public int subsetSum(int[] nums, int s) {
>     int[] dp = new int[s + 1]; 
>     dp[0] = 1;
>     for (int n : nums)
>         for (int i = s; i >= n; i--)
>             dp[i] += dp[i - n]; 
>     return dp[s];
> }
> ```

翻译以下就是：

原问题可以转化成寻找1个子集P，剩余的数取相反数组成集合N，满足sum(P) + sum(N) = target。

于是有：

```java
                  sum(P) - sum(N) = target
sum(P) + sum(N) + sum(P) - sum(N) = target + sum(P) + sum(N)
                       2 * sum(P) = target + sum(nums)
```

得： `sum(P) = (target + sum(nums)) / 2`

由观察得：**target + sum(nums) 必须是偶数**，这样可以加快判断。

这道题和我一篇关于**416题等分子集**的文章大同小异。

下面给出自底向上的DP解法：

dp [i] [j] : nums[1...i]的多少个组合能组成 j 。

1. dp [i] [j] = dp [i-1] [j-nums[i]] + dp [i-1] [j]
2. 基本条件：dp [0] [j] = 1
3. 通俗来说，dp [i] [j] 依赖其正上方元素和其正上方的前方的元素

```java
class Solution {
    public int findTargetSumWays(int[] nums, int S) {
        if(nums == null || nums.length == 0) {
            return 0;
        }
        int sum = 0;
        for(int num : nums) {
            sum += num;
        }
        if(S > sum || S < -sum || ((S + sum) & 1) > 0) {
            return 0;
        }
        int[] dp = new int[((S+sum) >>> 1) + 1];
        dp[0] = 1;
        for(int num : nums) {
            for(int j = dp.length - 1; j >= num; j--) {
                dp[j] += dp[j - num];
            }
            //比下面这种效率更高，更优雅
            // for(int j = dp.length - 1; j >= 0; j--) {
            //     dp[j] += j - num >= 0 ? dp[j - num] : 0;
            // }
        }
        return dp[dp.length - 1];
    }
}           
```

