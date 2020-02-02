#  438. Find All Anagrams in a String

1. 滑动窗口，空间O(1)，时间O(N)，这里可以用数组代替散列表。

   写得短一点，可能难懂，具体请看另一篇文章：字符串的滑动窗口。

   ```java
   public class Solution {
       public List<Integer> findAnagrams(String s, String t) {
           List<Integer> res = new LinkedList<>();
           if(t.length() > s.length()) {
               return res;
           }
           Integer[] map = new Integer[26];
           int counter = 0;
           for(char c : t.toCharArray()){
               if(map[c - 'a'] == null) {
                   map[c - 'a'] = 0;
                   counter++;
               }
               map[c - 'a']++;
           }
           
           int l = 0;
           int r = 0;
           while(r < s.length()) {
               
               char c = s.charAt(r);
               if(map[c - 'a'] != null && (--map[c - 'a']) == 0) {
                   counter--;
               }
               
               while(counter == 0) {
                   char ch = s.charAt(l);
                   if(map[ch - 'a'] != null && (map[ch - 'a']++) == 0) {
                       counter++;
                   }
                   if(r - l - 1 == t.length()) {
                       res.add(l);
                   }
                   l++;
               }
               r++;
           }
           return res;
       }
   }
   ```

   