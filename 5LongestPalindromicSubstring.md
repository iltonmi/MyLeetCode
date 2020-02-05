#  5. Longest Palindromic Substring

 https://leetcode.com/articles/longest-palindromic-substring/ 

 https://leetcode-cn.com/problems/longest-palindromic-substring/solution/zui-chang-hui-wen-zi-chuan-by-leetcode/  

1. 暴力，时间O(n^3), 空间O(1)，LeetCode上会超时，仅供观赏

   ```java
   class Solution {
       private int res = 0;
       public String longestPalindrome(String s) {
           if(s == null || s.length() < 1) {
               return s;
           }
           int low = 0;
           int high = 0;
           for(int i = 0; i < s.length(); i++) {
               for(int j = 0; j < s.length(); j++) {
                   if(isPalindrome(s, i, j)) {
                       if(res < j - i + 1) {
                           res = j - i + 1;
                           low = i;
                           high = j;
                       }
                   }
               }
           }
           return s.substring(low, high);
       }
       
       private boolean isPalindrome(String s, int i, int j) {
           if(s == null) {
               return true;
           }
           if(i > j || i < 0 || j > s.length() - 1) {
               return false;
           }
           while(i < j) {
               if(s.charAt(i) != s.charAt(j)) {
                   return false;
               }
           }
           return true;
       }
   }
   ```

   

2. dp矩阵，时间O(n^2), 空间O(n^2)

   ```java
   public String longestPalindrome(String s) {
       if(s == null || s.length() <= 1) {
           return s;
       }
       if(s.length() == 2) {
           return s.charAt(0) == s.charAt(1) ? s : s.substring(0, 1);
       }
       boolean[][] dp = new boolean[s.length()][s.length()];
       //初始化
       for(int i = 0; i < s.length(); i++) {
           dp[i][i] = true;
       }
       int low = 0;
       int high = 0;
       //初始化并记录
       for(int i = 1; i < s.length(); i++) {
           dp[i-1][i] = (s.charAt(i-1) == s.charAt(i));
           if(dp[i-1][i] && high - low < 1) {
                   low = i - 1;
                   high = i;
           }
       }
       //开始dp
       for(int j = 2; j < s.length() ; j++) {
           for(int i = 0; i <= j - 2; i++) {
               dp[i][j] = dp[i+1][j-1] && s.charAt(i) == s.charAt(j);
               if(dp[i][j] && high - low < j - i) {
                   low = i;
                   high = j;
               }
           }
       }
       return s.substring(low, high + 1);
   }
   ```

3. dp数组，时间O(n^2), 空间O(n)

   ```java
   public String longestPalindrome(String s) {
       if(s == null || s.length() <= 1) {
           return s;
       }
       boolean[] dp = new boolean[s.length()];
       //初始化
       for(int i = 0; i < s.length(); i++) {
           dp[i] = true;
       }
       int low = 0;
       int high = 0;
       for(int j = 1; j < s.length(); j++) {
           for(int i = 0; i <= j - 1; i++) {
               // if(i == j - 1) {
               //     dp[i] = (s.charAt(i) == s.charAt(j));
               // } else {
               //     dp[i] = dp[i+1] && (s.charAt(i) == s.charAt(j));
               // }
               //上面这一段注释掉的代码，逻辑是分开处理长度为2和长度大于2的情况
               //上面被注释掉的代码和下面这句等价
               dp[i] = dp[i+1] && (s.charAt(i) == s.charAt(j));
               if(dp[i] && high - low < j - i) {
                   low = i;
                   high = j;
               }
           }
       }
       return s.substring(low, high + 1);
   }
   ```

4. 中心扩展，时间O(n^2), 空间O(1)

   ```java
   class Solution {
       
       String p;
       
       public String longestPalindrome(String s) {
           if(s == null || s.length() <= 1) {
               return s;
           }
           this.p = s;
           int low = 0;
           int high = 0;
           for(int i = 0; i < s.length(); i++) {
               int res1 = expandAroundCenterToFindPalindrome(i, i);
               int res2 = expandAroundCenterToFindPalindrome(i, i+1);
               int curRes = Math.max(res1, res2);
               //长度对比
               if(curRes > high + 1 - low) {
                   //这个坐标需要斟酌，画图就容易理解
                   low = i - (curRes - 1) / 2;
                   high = i + curRes / 2;
               }
           }
           return s.substring(low, high + 1);
       }
       
       private int expandAroundCenterToFindPalindrome(int left, int right) {
           while(left >= 0 && right < p.length() && p.charAt(left) == p.charAt(right)) {
               left--;
               right++;
           }
           return right - left - 1;
       }
   }
   ```

   

5. Manacher's algorithm改版，时间O(n), 空间O(n)，这个时间复杂度的证明看另一篇文章

   ```java
   //改良位运算版
   public String longestPalindrome(String s) {
       if(s == null || s.length() == 0) {
           return "";
       }
       int[] pArr = new int[2 * s.length() + 1];
       int index = -1;
       int r = -1;
       int max = 0;
       for(int i = 0; i != pArr.length; i++) {
           //r = -1 < i = 0
           pArr[i] = r > i ? Math.min(pArr[(index << 1) - i], r - i) : 1;
           //left and right is the index of the char is going to be examined
           int left = i - pArr[i];
           int right = i + pArr[i];
           //(left & 1) == 0 相当于遇到分隔符、
           //非分隔符从manacher string到original string的映射是：i -> i >>> 1
          	//首先，非分隔符在manacher string中的所有肯定是奇数、
           //然后，我们来分析从original string到manacher string的映射
           //orgianl中索引i的字符前有i个字符，往这i个字符前方分别插入1个分隔符，总计插入i个字符
           //最后在索引i的字符前插入一个分隔符，总共插入i+1个字符。
           //所以origianl -> manacher = i -> 2i+1
           //因此manacher -> original = j -> j / 2 -> j >>>1
           while(left > -1 && right < pArr.length && 
                 ((left & 1) == 0 || (s.charAt(left >> 1) == s.charAt(right >> 1)))
                ) {
               pArr[i]++;
               left--;
               right++;
           }
           //index第一次是0
           if(i + pArr[i] > r) {
               r = i + pArr[i];
               index = i;
           }
           //max会被直接使用，所以必须>0
           if(pArr[i] > pArr[max]) {
               max = i;
           }
       }
    return s.substring((max - pArr[max] + 2) >> 1, (max + pArr[max]) >> 1);
   }
   //实际版
   class Solution {
       public String longestPalindrome(String s) {
           if(s == null || s.length() == 0) {
               return "";
           }
           int[] pArr = new int[2 * s.length() + 1];
           int index = -1;
           int r = -1;
           int max = 0;
           for(int i = 0; i != pArr.length; i++) {
               pArr[i] = r > i ? Math.min(pArr[(index << 1) - i], r - i) : 1;
               //left and right is the index of the char is going to be examined
               int left = i - pArr[i];
               int right = i + pArr[i];
               while(left > -1 && right < pArr.length &&
                       ((left & 1) == 0 ||
                        (s.charAt(((left + 1 ) >> 1) - 1) ==
                         s.charAt(((right + 1 ) >> 1) - 1)))
               ) {
                   pArr[i]++;
                   left--;
                   right++;
               }
               if(i + pArr[i] > r) {
                   r = i + pArr[i];
                   index = i;
               }
               if(pArr[i] > pArr[max]) {
                   max = i;
               }
           }
           return s.substring(((max - pArr[max] + 3) >> 1) - 1,
                              ((max + pArr[max] + 1) >> 1) - 1);
       }
   }
   ```
   
   