# Longest Common String

1. 经典dp，空间复杂度O(M * N)，时间复杂度O(M * N)。

   dp [i] [j] : str1[0 .. i] 和 str2[0 .. j]的必须包含str1[i]和str2[j]的最长公共子串。

   ```java
   public static int[][] getdp(char[] str1, char[] str2) {
       int[][] dp = new int[str1.length][str2.length];
       for (int i = 0; i < str1.length; i++) {
           if (str1[i] == str2[0]) {
               dp[i][0] = 1;
           }
       }
       for (int j = 1; j < str2.length; j++) {
           if (str1[0] == str2[j]) {
               dp[0][j] = 1;
           }
       }
       for (int i = 1; i < str1.length; i++) {
           for (int j = 1; j < str2.length; j++) {
               if (str1[i] == str2[j]) {
                   dp[i][j] = dp[i - 1][j - 1] + 1;
               }
           }
       }
       return dp;
   }
   
   public static String lcst1(String str1, String str2) {
       if (str1 == null || str2 == null || str1.equals("") || str2.equals("")) {
           return "";
       }
       char[] chs1 = str1.toCharArray();
       char[] chs2 = str2.toCharArray();
       int[][] dp = getdp(chs1, chs2);
       int end = 0;
       int max = 0;
       for (int i = 0; i < chs1.length; i++) {
           for (int j = 0; j < chs2.length; j++) {
               if (dp[i][j] > max) {
                   end = i;
                   max = dp[i][j];
               }
           }
       }
       return str1.substring(end - max + 1, end + 1);
   }
   ```

   

2. 空间复杂度O(1)，时间复杂度O(M * N)。，因为每个dp[i] [j] 只依赖其左上角的值，因此按照从左上到右下的方向沿着斜线计算，只需要一个变量就可以计算出所有位置的值。

   ```java
   public static String lcst2(String str1, String str2) {
       if (str1 == null || str2 == null || str1.equals("") || str2.equals("")) {
           return "";
       }
       char[] chs1 = str1.toCharArray();
       char[] chs2 = str2.toCharArray();
       int row = 0; // 斜线开始位置的行
       int col = chs2.length - 1; // 斜线开始位置的列
       int max = 0; // 记录最大长度
       int end = 0; // 最大长度更新时，记录子串的结尾位置
       while (row < chs1.length) {
           int i = row;
           int j = col;
           int len = 0;
           // 从(i,j)开始向右下方遍历
           while (i < chs1.length && j < chs2.length) {
               if (chs1[i] != chs2[j]) {
                   len = 0;
               } else {
                   len++;
               }
               // 记录最大值及相应结束字符的位置
               if (len > max) {
                   end = i;
                   max = len;
               }
               i++;
               j++;
           }
           if (col > 0) { // 斜线开始位置的列先向左移动
               col--;
           } else { // 列移动到最左之后，行向下移动
               row++;
           }
       }
       return str1.substring(end - max + 1, end + 1);
   }
   ```

   