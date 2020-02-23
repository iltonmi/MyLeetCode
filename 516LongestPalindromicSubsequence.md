# 516. Longest Palindromic Subsequence

 https://leetcode.com/problems/longest-palindromic-subsequence/discuss/99101/Straight-forward-Java-DP-solution 

>  `dp[i][j]`: the longest palindromic subsequence's length of substring(i, j), here i, j represent left, right indexes in the string
> `State transition`:
> `dp[i][j] = dp[i+1][j-1] + 2` if s.charAt(i) == s.charAt(j)
> otherwise, `dp[i][j] = Math.max(dp[i+1][j], dp[i][j-1])`
> `Initialization`: `dp[i][i] = 1` 

1. 记忆化自顶向下DP，空间O(N ^ 2)，时间O(N ^ 2)

   ```java
   private Integer[][] dp;
   public int longestPalindromeSubseq(String s) {
       this.dp = new Integer[s.length()][s.length()];
       return helper(s, 0, s.length() - 1);
   }
   
   private int helper(String s, int i, int j) {
       if (dp[i][j] != null) {
           return dp[i][j];
       }
       if (i > j) {
           return 0;
       }
       if (i == j) {
           return 1;
       }
   
       if (s.charAt(i) == s.charAt(j)) {
           dp[i][j] = helper(s, i + 1, j - 1) + 2;
       } else {
           dp[i][j] = Math.max(helper(s, i + 1, j), helper(s, i, j - 1));
       }
       return dp[i][j];
   }
   ```

   

2. 自底向上DP，空间O(N ^ 2)，时间O(N ^ 2)

   ```java
   public int longestPalindromeSubseq(String s) {
       int[][] dp = new int[s.length()][s.length()];
   
       for (int i = s.length() - 1; i >= 0; i--) {
           dp[i][i] = 1;
           for (int j = i + 1; j < s.length(); j++) {
               if (s.charAt(i) == s.charAt(j)) {
                   dp[i][j] = dp[i+1][j-1] + 2;
               } else {
                   dp[i][j] = Math.max(dp[i+1][j], dp[i][j-1]);
               }
           }
       }
       return dp[0][s.length()-1];
   }
   ```

3. 自底向上DP，空间O(N)，时间O(N ^ 2)。略。