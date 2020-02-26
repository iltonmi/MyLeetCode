# 424. Longest Repeating Character Replacement

1. 滑动窗口，空间O(1), 时间O(N)

   ```java
   public int characterReplacement(String s, int k) {
       int len = s.length();
       int[] count = new int[26];
       int start = 0;
       //窗口中数量最多的字符
       int maxCount = 0;
       int res = 0;
       for (int end = 0; end < len; end++) {
           //同一个窗口中，同类字符数量越大，需要替换的地方越少  
           maxCount = Math.max(maxCount, ++count[s.charAt(end) - 'A']);
           //需要修改的地方数量大于k，窗口左边滑动缩小窗口，修正
           while (end - start + 1 - maxCount > k) {
               count[s.charAt(start) - 'A']--;
               start++;
           }
           //记录结果
           res = Math.max(res, end - start + 1);
       }
       return res;
   }
   ```

   

2. 待定 