#  28. Implement strStr()

子字符串匹配。

1. 暴力，空间O(1)，时间O(N * M)

   ```java
   class Solution {
       public int strStr(String haystack, String needle) {
           if("".equals(needle)) {
               return 0;
           }
           int l = 0;
           int r = 0;
           while(r <= haystack.length() - needle.length()) {
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

   

2. KMP，空间O(M)，时间O(N)

   ```java
   public int strStr(String s, String m) {
       if (s == null || m == null || s.length() < m.length()) {
           return -1;
       }
       //和KMP文章唯一不同的地方
       //根据题目定义，m.length() == 0，返回0
       if(m.length() < 1) {
           return 0;
       }
       char[] ss = s.toCharArray();
       char[] ms = m.toCharArray();
       int si = 0;
       int mi = 0;
       int[] next = getNextArray(ms);
       while (si < ss.length && mi < ms.length) {
           if (ss[si] == ms[mi]) {
               si++;
               mi++;
           } else if (next[mi] == -1) {
               si++;
           } else {
               mi = next[mi];
           }
       }
       return mi == ms.length ? si - mi : -1;
   }
   
   public int[] getNextArray(char[] ms) {
       if (ms.length == 1) {
           return new int[] { -1 };
       }
       int[] next = new int[ms.length];
       next[0] = -1;
       next[1] = 0;
       int pos = 2;
       int cn = 0;    
       while (pos < next.length) {
           if (ms[pos - 1] == ms[cn]) {
               next[pos++] = ++cn;
           } else if (cn > 0) {
               cn = next[cn];
           } else {
               next[pos++] = 0;
           }
       }
       return next;
   }
   ```

   