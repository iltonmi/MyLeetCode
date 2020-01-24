#  221. Maximal Square 

第一感觉是DP，当然这次的暴力解法也很值得学习。



1. 暴力，遍历整个数组，去搜索以当前元素为左上角的最大的正方形（遍历对角线元素，检查对角线元素所在的行和列）。时间O((MN) ^ 2)，空间O(1)。

   ```java
   class Solution {
       public int maximalSquare(char[][] matrix) {
           int rows = matrix.length;
           int cols = rows > 0 ? matrix[0].length : 0;
           int res = 0;
           for(int i = 0; i < rows; i++) {
               for(int j = 0; j < cols; j++) {
                   if(matrix[i][j] == '0') {
                       continue;
                   }
                   int sqLen = 1;
                   boolean flag = true;
                   while(flag && i + sqLen < rows && j + sqLen < cols) {
                       for(int k = j; k <= j + sqLen; k++) {
                           if(matrix[i + sqLen][k] == '0') {
                               flag = false;
                               break;
                           }
                       }
                       for(int k = i; k <= i + sqLen; k++) {
                           if(matrix[k][j + sqLen] == '0') {
                               flag = false;
                               break;
                           }
                       }
                       if(flag) {
                           sqLen++;
                       }
                   }
                   if(sqLen > res) {
                       res = sqLen;
                   }
               }
           }
           
           return res * res;
       }
   }
   ```

   

2. DP，时间O(MN)，空间O(MN)，使用了哨兵。

   重要的是状态转移方程。
   $$
   dp[i][j]的定义：以(i,j)为右下角的正方形的大小。
   $$

   $$
   dp[i][j]上方和左方的元素的大小不一时，dp[i][j] = 1 + min(dp[i-1][j],dp[i][j-1])
   $$

   $$
   dp[i][j]上方和左方的元素的大小相等时,还要考虑左上方的元素,1 + dp[i][j] = min(dp[i-1][j],dp[i][j-1], dp[i-1][j-1])
   $$

   这2种情况通过作图可以轻松看出。

   2种情况合并也不影响结果正确性，因此：
   $$
   dp[i][j]=1+min(dp[i-1][j-1], dp[i-1][j],dp[i][j-1])
   $$
   

   ```java
   class Solution {
       public int maximalSquare(char[][] matrix) {
           int rows = matrix.length;
           int cols = rows > 0 ? matrix[0].length : 0;
           int res = 0;
           int[][] dp = new int[rows + 1][cols + 1];
           //多出来的一行一列当作哨兵
           for(int i = 1; i <= rows; i++) {
               for(int j = 1; j <= cols; j++) {
                   if(matrix[i-1][j-1] == '0') {
                       continue;
                   }
                   dp[i][j] = 1 + Math.min(dp[i-1][j-1],
                                           Math.min(dp[i-1][j], dp[i][j-1]));
                   res = dp[i][j] > res ? dp[i][j] : res;
               }
           }
           return res * res;
       }
   }
   ```

   

3. 优化DP，时间O(MN)，空间O(N)。

   原理：将矩阵压缩成一维数组。

   ​			从DP可知，状态转移方程需要3个变量。

   ​			通过一维数组，更新 dp[j] 时，dp[j] 本身的值等价于二维数组中上方的元素，dp[j-1] 的值等价于二维数组中左方的元素，而左上方的元素则需要开辟一个变量进行保存。

   ​			同样的可以通过哨兵值简化代码。

   ```java
   class Solution {
       public int maximalSquare(char[][] matrix) {
           int rows = matrix.length;
           int cols = rows > 0 ? matrix[0].length : 0;
           int res = 0;
           int[] dp = new int[cols + 1];
           for(int i = 1; i <= rows; i++) {
               int topR = 0;
               for(int j = 1; j <= cols; j++) {
                   int temp = dp[j];
                   if(matrix[i-1][j-1] == '0') {
                       dp[j] = 0;
                   } else {
                       dp[j] = 1 + Math.min(topR,
                                               Math.min(dp[j], dp[j - 1]));
                       res = dp[j] > res ? dp[j] : res;   
                   }
                   topR = temp;
               }
           }
           return res * res;
       }
   }
   ```

   