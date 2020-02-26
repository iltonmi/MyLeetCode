# 340 Longest Substring with At Most K Distinct Characters

1. 散列表，滑动窗口，空间O(K)，时间O(N)。

   用整型数组做更快。

   ```java
   public int lengthOfLongestSubstringKDistinct(String s, int k) {
       int n = s.length();
       if (n * k == 0) {
           return 0;
       }
   
       int count = 0;
       HashMap<Character, Integer> map = new HashMap<Character, Integer>(k+1);
       int res = 0;
       for(int right = 0; right < n; right++) {
           char ch = s.charAt(right);
           map.put(ch, map.getOrDefault(ch, 0) + 1);
           count++;
           if (map.size() <= k) {
               res = Math.max(res, count);
               continue;
           }
           for(int l = right - count + 1; map.size() > k; l++) {
               char temp = s.charAt(l);
               int times = map.get(temp);
               if(times == 1) {
                   map.remove(temp);
               } else {
                   map.put(temp, times - 1);
               }
               count--;
           }
       }
       return res;
   }
   ```

   