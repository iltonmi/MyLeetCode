#  125. Valid Palindrome

双指针，字符和ascii码

1. 时间O(N)，空间O(1)

   ```java
   class Solution {
       public boolean isPalindrome(String s) {
           if(s.length() == 0) {
               return true;
           }
           int l = 0;
           int r = s.length() - 1;
           while(l <= r) {
               while(!isLetterOrDigit(s.charAt(l)) && l < r) {
                   l++;
               }
               while(!isLetterOrDigit(s.charAt(r)) && r > l) {
                   r--;
               }
               if(!equalsIgnoreCases(s.charAt(l), s.charAt(r))) {
                   return false;
               }
               l++;
               r--;
           }
           return true;
       }
       
       //前置条件 ch1 and ch2 are letter or digit
       //如果是数字，ch1 - 'a' == ch2 - 'a'也可以判断
       private boolean equalsIgnoreCases(char ch1, char ch2) {
           return (isUpperCase(ch1) ? ch1 - 'A' : ch1 -'a') 
               == (isUpperCase(ch2) ? ch2 - 'A' : ch2 -'a'); 
       }
       
       private boolean isLetterOrDigit(char ch) {
           return isDigit(ch) || isLetter(ch);
       }
       
       private boolean isDigit(char ch) {
           return (ch - '0' < 10 && ch - '0' >= 0) ;
       }
       
       private boolean isLetter(char ch) {
           return isLowerCase(ch) || isUpperCase(ch);
       }
       
       private boolean isLowerCase(char ch) {
           return (ch - 'a' < 26 && ch - 'a' >= 0);
       }
       
       private boolean isUpperCase(char ch) {
           return (ch - 'A' < 26 && ch - 'A' >= 0);
       }
   }
   ```

   

2. 用图，时间O(N)，空间O(128)

   ```java
   class Solution {
       //map[i] == 0 代表不是字母或数字
       //map[i] == 1...10 代表 0...9
       //map[i] == 11...36 代表 a...z
       //令A...Z的数值等于a...z即可
       private static final int[] map = new int[128];
       static {
           for(int i=0; i<10; i++) {
               map[i+'0'] = (i+1);
           }
           for(int i=0; i<26; i++) {
               map[i+'a'] = (i+11);
               map[i+'A'] = (i+11);
           }
       }
       
       public boolean isPalindrome(String s) {
           char[] ss = s.toCharArray();
           
           int i = 0;
           int j = ss.length - 1;
           while(i < j) {
               int ci = map[ss[i]];
               int cj = map[ss[j]];
               
               if(ci!=0 && cj!=0) {
                   if(ci != cj) {
                       return false;
                   }
                   i++;
                   j--;
               } else {
                   if(ci == 0) {
                       i++;
                   }
                   if(cj == 0) {
                       j--;
                   }
               }
           }
           return true;
       }
   }
   ```

   