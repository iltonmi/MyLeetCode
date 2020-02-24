#  1143. Longest Common Subsequence 

空间O(M * N)，时间O(M * N)，证明见关于题1312的文章。

```java
public int longestCommonSubsequence(String text1, String text2) {
    if(text1 == null || text2 == null 
       || text1.length() == 0 || text2.length() == 0) {
        return 0;
    }
    int m = text1.length();
    int n = text2.length();
    char[] chs1 = text1.toCharArray();
    char[] chs2 = text2.toCharArray();
    int[][] dp = new int[m + 1][n + 1];
    for(int i = 0; i < m; i++) {
        for(int j = 0; j < n; j++) {
            dp[i+1][j+1] = chs1[i] == chs2[j] 
               ? dp[i][j] + 1 : Math.max(dp[i+1][j], dp[i][j+1]);
        }
    }
    return dp[m][n];
}
```

