#  139. Word Break ，复杂度待定

Word break make my world break!

 https://leetcode-cn.com/problems/word-break/solution/dan-ci-chai-fen-by-leetcode/ 

1. 递归回溯，复杂度待定

   匹配前缀，若前缀在字典中，则递归匹配剩余部分的前缀。

   ```java
   class Solution {
       private String s;
       private Set<String> set;
       public boolean wordBreak(String s, List<String> wordDict) {
           this.s = s;
           this.set = new HashSet<>(wordDict);
           return wb(0);
       }
       
       private boolean wb(int start) {
           if(start == s.length()) {
               return true;
           }
           for(int end = start + 1; end <= s.length(); end++) {
               if(set.contains(s.substring(start, end)) && wb(end)) {
                   return true;
               }
           }
           return false;
       }
   }
   ```

   

2. 记忆化回溯，复杂度待定

   ```java
   class Solution {
       private String s;
       private Set<String> set;
       private Boolean[] help;
       public boolean wordBreak(String s, List<String> wordDict) {
           this.s = s;
           this.set = new HashSet<>(wordDict);
           this.help = new Boolean[s.length()];
           return wb(0);
       }
       
       private boolean wb(int start) {
           if(start == s.length()) {
               return true;
           }
           if(help[start] != null) {
               return help[start];
           }
           for(int end = start + 1; end <= s.length(); end++) {
               if(set.contains(s.substring(start, end)) && wb(end)) {
                   return help[start] = true;
               }
           }
           return help[start] = false;
       }
   }
   ```

   

3. 宽度优先搜索

   ```java
   class Solution {
       public boolean wordBreak(String s, List<String> wordDict) {
           Queue<Integer> queue = new LinkedList<>();
           Set<String> set = new HashSet<>(wordDict);
           boolean[] visited = new boolean[s.length()];
           queue.add(0);
           while(!queue.isEmpty()) {
               Integer start = queue.poll();
               if(visited[start] == false) {
                   for(int end = start + 1; end <= s.length(); end++) {
                       if(set.contains(s.substring(start, end))) {
                           if(end == s.length()) {
                               return true;
                           }
                           queue.add(end);
                       }
                   }
                   visited[start] = true;
               }
           }
           return false;
       }
   }
   ```

   

4. DP