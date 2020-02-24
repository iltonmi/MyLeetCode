# 72. Edit Distance

> There are three pairs of shorter strings after the last operation, corresponding to the strings after a match/substitution, insertion, or deletion. *If* we knew the cost of editing these three pairs of smaller strings, we could decide which option leads to the best solution and choose that option accordingly.

本质在于：根据str1[1 .. i-1]和str2[1 .. j-1]的情况，去决定最后一个字符的进行替换、插入和删除之中的哪一种操作。

> More precisely, let D [ i , j ] be the minimum number of differences between P1, P2, ... , Pi and the segment of *T* ending at *j*. 
>
> D[ i , j ] is the *minimum* of the three possible ways to extend smaller strings.

**the minimum number of differences**：差别的最小数值，可以理解为操作数最小的一组操作。

**smaller strings**：指的是3种操作对应的更短的字符串 pair。

**需要决定一个方向：将str1变为str2的最小代价，抑或相反**

实现代码如下（方便起见，将str2变为str1）：

1. 递归（栈溢出。。。。），空间复杂度O(min(M, N))，时间复杂度O(3 ^ N)。

   ```java
   public int minDistance(String word1, String word2) {
       //dp[i][j]: least number of edit operations of word1[0..i] and word2[0..j]
       //if word1[i] = word2[j], dp[i][j] = dp[i-1][j-1]
       //if word1[i] != word2[j], dp[]
       return helper(word1, word2, word1.length() - 1, word2.length() - 1);
   }
   
   private int helper(String str1, String str2, int i, int j) {
       if(i == 0) {
           return str2.length();
       }
       if(j == 0) {
           return str1.length();
       }
       //对于替换：如果i,j一样，就不需要替换；否则替换，多了1个不同之处。
       int match = helper(str1, str2, i - 1, j - 1) 
           + (str1.charAt(i) == str2.charAt(j) ? 0 : 1);
       //往str2[0..j]后面插入一个str1[i]
       int insert = helper(str1, str2, i - 1, j) + 1;
       //从str2[0..j]后面删除str2[j]
       int delete = helper(str1, str2, i, j - 1) + 1;
       return Math.min(match, Math.min(insert, delete));
   }
   ```

   

2. 自底向上 ，空间复杂度O(M * N)，时间复杂度O(M * N)。

   ```java
   public int minDistance(String word1, String word2) {
       int m = word1.length();
       int n = word2.length();
       if(m == 0) {
           return n;
       }
       if(n == 0) {
           return m;
       }
       int[][] dp = new int[m+1][n+1];
       for(int i = 1; i < dp.length; i++) {
           dp[i][0] = i;
       }
   
       for(int j = 1; j < dp[0].length; j++) {
           dp[0][j] = j;
       }
   
       for(int i = 1; i < dp.length; i++) {
           for(int j = 1; j < dp[0].length; j++) {
               int match = dp[i-1][j-1] 
                   + (word1.charAt(i-1) == word2.charAt(j-1) ? 0 : 1);
               int insert = dp[i][j-1] + 1;
               int delete = dp[i-1][j] + 1;
               dp[i][j] = Math.min(match, Math.min(insert, delete));
           }
       }
       return dp[m][n];
   }
   ```

   

3. 空间压缩，空间复杂度O(min(M, N))，时间复杂度O(M * N)。

   ```java
   public int minDistance(String word1, String word2) {
       int m = word1.length();
       int n = word2.length();
       //保证word2更短，删了也不影响
       if(m < n) {
           int tempLen = m;
           m = n;
           n = tempLen;
           String tempStr = word1;
           word1 = word2;
           word2 = tempStr;
       }
       if(m == 0) {
           return n;
       }
       if(n == 0) {
           return m;
       }
       int[] dp = new int[n+1];
       for(int j = 1; j < dp.length; j++) {
           dp[j] = j;
       }
       for(int i = 1; i <= m; i++) {
           int pre = i - 1;
           dp[0] = i;
           for(int j = 1; j <= n; j++) {
               int match = pre
                   + (word1.charAt(i-1) == word2.charAt(j-1) ? 0 : 1);
               int insert = dp[j-1] + 1;
               int delete = dp[j] + 1;
               pre = dp[j];
               dp[j] = Math.min(match, Math.min(insert, delete));
           }
       }
       return dp[n];
   }
   ```

   