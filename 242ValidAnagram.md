# 242. Valid Anagram

集邮。

1. 排序，空间时间取决于排序算法。

   ```java
   public boolean isAnagram(String s, String t) {
       if (s.length() != t.length()) {
           return false;
       }
       char[] str1 = s.toCharArray();
       char[] str2 = t.toCharArray();
       Arrays.sort(str1);
       Arrays.sort(str2);
       return Arrays.equals(str1, str2);
   }
   ```

   

2. 散列表，空间O(S)，时间O(N)。S是字符集大小。

   ```java
   public boolean isAnagram(String s, String t) {
       if (s.length() != t.length()) {
           return false;
       }
       int[] table = new int[26];
       for (int i = 0; i < s.length(); i++) {
           table[s.charAt(i) - 'a']++;
       }
       for (int i = 0; i < t.length(); i++) {
           table[t.charAt(i) - 'a']--;
           if (table[t.charAt(i) - 'a'] < 0) {
               return false;
           }
       }
       return true;
   }
   ```

   