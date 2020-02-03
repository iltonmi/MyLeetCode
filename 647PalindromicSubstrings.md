# 647. Palindromic Substrings



1. 中心扩展，空间O(1)，时间O(N ^ 2)

   ```java
   public int countSubstrings(String S) {
       int N = S.length(), ans = 0;
       for (int center = 0; center <= 2*N-1; ++center) {
           int left = center / 2;
           int right = left + center % 2;
           while (left >= 0 && right < N && S.charAt(left) == S.charAt(right)) {
               ans++;
               left--;
               right++;
           }
       }
       return ans;
   }
   ```

   

2. 马拉车 ，空间O(N)，时间O(N)

   1. 不生成马拉车字符串

      ```java
      public int countSubstrings(String S) {
          if(S == null || S.length() == 0) {
              return 0;
          }
          int res = 0;
          int len = S.length();
          int[] p = new int[2 * len + 1];
          int index = -1;
          int r = -1;
          for(int i = 0; i < p.length; i++) {
              p[i] = r > i ? Math.min(p[2 * index - i], r - i) : 1;
      
              int left = i - p[i];
              int right = i + p[i];
              //索引i是第i+1个字符，前面i个字符的每个前面插入1个分隔符，插入i个，
              //自身前面插入分隔符1个，于是前面插入了i+1个。
              //索引从i -> i+(i+1) = 2i+1
              // i -> 
              while(left > -1 && right < p.length 
                    && ((left & 1) == 0 
                        || S.charAt(left >>> 1) == S.charAt(right >>> 1)) ) {
                  p[i]++;
                  left--;
                  right++;
              }
      
              if(i + p[i] > r) {
                  r = i + p[i];
                  index = i;
              }
      
              res += (p[i] >>> 1);
          }
          return res;
      }
      ```

      

   2. 生成马拉车字符串

   ```java
   class Solution {
       public int countSubstrings(String S) {
        if(S == null || S.length() == 0) {
               return 0;
           }
           int res = 0;
           int len = S.length();
           int[] p = new int[2 * len + 1];
           char[] ms = getManacherString(S);
           int index = -1;
           int r = -1;
           for(int i = 0; i < p.length; i++) {
               p[i] = r > i ? Math.min(p[2 * index - i], r - i) : 1;
   
               int left = i - p[i];
               int right = i + p[i];
               while(left > -1 && right < p.length && ms[left] == ms[right] ) {
                   p[i]++;
                   left--;
                   right++;
               }
   
               if(i + p[i] > r) {
                   r = i + p[i];
                   index = i;
               }
   
               res += (p[i] >>> 1);
           }
           return res;
       }
       
       private char[] getManacherString(String s) {
           int len = s.length();
           char[] ms = new char[2 * len + 1];
           for(int i = 0; i < ms.length; i++) {
               ms[i] = (i & 1) == 0 ? '#' : s.charAt(i >>> 1);
           }
           return ms;
       }
   }
   ```
   
   