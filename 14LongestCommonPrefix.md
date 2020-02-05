#  14. Longest Common Prefix

 https://leetcode.com/articles/longest-common-prefix/ 

1. 逐个单词比较（水平比较），空间O(1)，时间O(S)。S是所有字符串的总长度。

   ```java
   public String longestCommonPrefix(String[] strs) {
       if (strs.length == 0) return "";
       String prefix = strs[0];
       for (int i = 1; i < strs.length; i++) {
           //return the first occurrence
           //返回0说明string以prefix开头
           while (strs[i].indexOf(prefix) != 0) {
               //prefix长度减小1
               prefix = prefix.substring(0, prefix.length() - 1);
               if (prefix.isEmpty()) {
                   return "";
               }
           }
       }
       return prefix;
   }
   ```

   

2. 逐个字母比较（垂直比较），空间O(1)，时间O(S)。

   ```java
   public String longestCommonPrefix(String[] strs) {
       if (strs == null || strs.length == 0) {
           return "";
       }
       for (int i = 0; i < strs[0].length() ; i++){
           char c = strs[0].charAt(i);
           for (int j = 1; j < strs.length; j ++) {
               if (i == strs[j].length() || strs[j].charAt(i) != c) {
                   //charAt(i)越界或不等，前缀不包括i
                   return strs[0].substring(0, i); 
               }        
           }
       }
       return strs[0];
   }
   ```

   

3. 分而治之（递归），两两比较，空间O(MlogN)，时间O(S)，M是某个字符串的长度。

   ```java
   class Solution {
       public String longestCommonPrefix(String[] strs) {
           if (strs == null || strs.length == 0) {
                return "";
           }    
           return longestCommonPrefix(strs, 0 , strs.length - 1);
       }
   
       private String longestCommonPrefix(String[] strs, int l, int r) {
           if (l == r) {
               return strs[l];
           }
           else {
               int mid = (l + r) / 2;
               String lcpLeft =   longestCommonPrefix(strs, l , mid);
               String lcpRight =  longestCommonPrefix(strs, mid + 1,r);
               return commonPrefix(lcpLeft, lcpRight);
           }
       }
   
       private String commonPrefix(String left,String right) {
           int min = Math.min(left.length(), right.length());       
           for (int i = 0; i < min; i++) {
               if ( left.charAt(i) != right.charAt(i) ) {
                   return left.substring(0, i);
               }
           }
           return left.substring(0, min);
       }
   }
   ```

   

4. 二分查找。。。感觉这个方法为了一题多解强行提出。。。

5. 前缀树，空间O(S)，时间O(S)，其中：建立前缀树耗时O(S)，查找公告前缀耗时O(M)。

   前缀树的建立请看208Trie的文章。

   ```java
   //返回 word 和前缀树字典中所有字符串的最长公共前缀
   private String searchLongestPrefix(String word) {
       TrieNode node = root;
       StringBuilder prefix = new StringBuilder();
       for (int i = 0; i < word.length(); i++) {
           char curLetter = word.charAt(i);
           //若当前字符是某个字符串的最后一个字符，
           //或前缀树有多条link，说明从下一个字符必定不是公告字符串的一部分
           if (node.containsKey(curLetter) 
               && (node.getLinks() == 1) && (!node.isEnd())) {
               
               prefix.append(curLetter);
               node = node.get(curLetter);
           } else {
               return prefix.toString();
           }
        }
        return prefix.toString();
   }
   ```

   