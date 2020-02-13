#  387. First Unique Character in a String

这题，必须得至少遍历一次才能知道字符出现次数，双指针也不太行，也没找到特定算法。

1. 散列，空间O(S), 时间O(N)。

   ```java
   public int firstUniqChar(String s) {
       if(s == null) {
           return -1;
       }
       int[] map = new int[26];
       for(int i = 0; i < s.length(); i++) {
           map[s.charAt(i) - 'a']++;
       }
       for(int i = 0; i < s.length(); i++) {
           if(map[s.charAt(i) - 'a'] == 1) {
               return i;
           }
       }
       return -1;
   }
   ```

   