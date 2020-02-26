#  3. Longest Substring Without Repeating Characters 

 https://leetcode.com/articles/longest-substring-without-repeating-characters/ 

 https://leetcode-cn.com/problems/longest-substring-without-repeating-characters/solution/wu-zhong-fu-zi-fu-de-zui-chang-zi-chuan-by-leetcod/ 

**注意：LeetCode官方题解很多时候会为了简洁性、性能，去过度牺牲可读性，有时候感觉矫枉过正了。例如这道题的题解，后面使用滑动窗口时，可能会破坏滑动窗口左闭右开的定义。**

1. 暴力枚举所有可能的子串，HashSet判断，时间O(n^3), 空间O(min(m, n))，m是包含字符串中所有字符的字符表大小，LeetCode上会超时，仅供观赏

   ```java
   class Solution {
       String p;
       public int lengthOfLongestSubstring(String s) {
           int res = 0;
           if(s == null) {
               return res;
           }
           this.p = s;
           for(int i = 0; i < s.length(); ++i) {
               for(int j = i; j < s.length(); ++j) {
                   if(allUnique(i, j)) {
                       res = Math.max(res, j - i + 1);
                   }
               }
           }
           return res;
       }
       
       private boolean allUnique(int from, int to) {
           Set<Character> set = new HashSet<>();
           for(int i = from; i <= to; i++) {
               Character ch = p.charAt(i);
               if(set.contains(ch)) {
                   return false;
               }
               set.add(ch);
           }
           return true;
       }
   }
   ```

   

2. 使用HashSet作滑动窗口，时间O(2n) =O(n), 空间O(min(m, n))，m是包含字符串中所有字符的字符表大小

   ```java
   class Solution {
       public int lengthOfLongestSubstring(String s) {
           int res = 0;
           if(s == null) {
               return res;
           }
           int i = 0;
           int j = 0;
           Set<Character> set = new HashSet<>();
           while(i < s.length() && j < s.length()) {
               if(!set.contains(s.charAt(j))) {
                   set.add(s.charAt(j++));
                   res = Math.max(res, j - i);
               } else {
                   set.remove(s.charAt(i++));
               }
           }
           return res;
       }
   }
   ```

   

3. 使用HashMap作滑动窗口，时间O(n), 空间O(min(m, n))，m是包含字符串中所有字符的字符表大小。

   相比HashSet做作滑动窗口，减少了n次循环。

   ```java
   public static int lengthOfLongestSubstring(String s) {
       int res = 0;
       if(s == null || s.length() == 0) {
           return res;
       }
       Map<Character, Integer> map = new HashMap<>();
       map.put(s.charAt(0), 0);
       res = 1;
       for(int j = 1, i = 0;  j < s.length(); j++) {
           if(map.containsKey(s.charAt(j))) {
               //因为s.charAt(j)可能小于i, 而没有被清除出去
               //比如12321，窗口缩窄到32，第二次遇到1时，1不在窗口，但仍存在其记录
               i = Math.max(map.get(s.charAt(j)) + 1, i);
           }
           res = Math.max(res, j - i + 1);
           map.put(s.charAt(j), j);
       }
       return res;
   }
   ```

   

4.  **Java (Assuming ASCII 128)** ，在已知字符集的情况下

   The previous implements all have no assumption on the charset of the string `s`.

   If we know that the charset is rather small, we can replace the `Map` with an integer array as direct access table.

   Commonly used tables are:

   - `int[26]` for Letters 'a' - 'z' or 'A' - 'Z'
   - `int[128]` for ASCII
   - `int[256]` for Extended ASCII

   ```java
   public static int lengthOfLongestSubstring(String s) {
       int res = 0;
       if(s == null || s.length() == 0) {
           return res;
       }
       int[] map = new int[128];
       Arrays.fill(map, -1);
       map[s.charAt(0)] = 0;
       res = 1;
       for(int j = 1, i = 0;  j < s.length(); j++) {
           if(map[s.charAt(j)] != -1) {
               //因为s.charAt(j)可能小于i, 而没有被清除出去
               i = Math.max(map[s.charAt(j)] + 1, i);
           }
           res = Math.max(res, j - i + 1);
           map[s.charAt(j)] = j;
       }
       return res;
   }
   ```

   