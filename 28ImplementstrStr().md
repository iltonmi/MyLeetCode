#  28. Implement strStr()

子字符串匹配。

1. 暴力

   ```java
   class Solution {
       public int strStr(String haystack, String needle) {
           if("".equals(needle)) {
               return 0;
           }
           int l = 0;
           int r = 0;
           while(r < haystack.length()) {
               while(r < haystack.length() && r - l + 1 <= needle.length() 
                     && haystack.charAt(r) == needle.charAt(r - l)) {
                   
                   if(r - l + 1 == needle.length()) {
                       return l;
                   }
                   r++;
               }
               l++;
               r = l;
           }
           return -1;
       }
   }
   ```

   ```java
   public int strStr(String haystack, String needle) {
       if(needle.length() == 0) {
           return 0;
       }
       int M = needle.length();
       int N = haystack.length();
       for (int i = 0; i <= N - M; i++) { 
           int j;
           for (j = 0; j < M && haystack.charAt(i+j) == needle.charAt(j); j++) {}
           if (j == M) {
               return i;
           }
       }
       return -1;
   }
   ```

   

2. KMP

   