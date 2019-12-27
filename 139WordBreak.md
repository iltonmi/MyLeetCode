#  139. Word Break ，慢慢消化

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

   

3. 宽度优先搜索，时间O(N^2), 空间O(N)

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

   

4. DP，时间O(N^2), 空间O(N)

   将问题分解成子问题，举例说明：

   1. 假设有字符串s，其值为“catsanddogs”
   2. 若s[0, j) = "cats“ 和 s[j, i) = "and" 都能由字典中的单词组成, 则s[0, i) = "catsand" 也可以由字典中的单词组成。

   代码如下：

   ```java
   class Solution {
       public boolean wordBreak(String s, List<String> wordDict) {
           Set<String> set = new HashSet<>(wordDict);
           if(set.contains(s)) {
               return true;
           }
           boolean[] dp = new boolean[s.length() + 1];
           dp[0] = true;
           //外循环，字符s[0,1) -> s[0, s.length())
           for(int i = 1; i <= s.length(); i++) {
               //内循环，dp[0] -> dp[i-1]，dp[i]不包含i
               //此处无法检查dp[i]，因为dp[i]正是我们需要判断的值
               //存在问题：会漏检s.substring(0, s.length())
               for(int j = 0; j < i; j++) {
                   if(dp[j] && set.contains(s.substring(j, i))) {
                       dp[i] = true;
                       break;
                   }
               }
           }
           return dp[s.length()];
       }
   }
   ```

   