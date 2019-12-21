# 49. Group Anagrams

这道题考察的是HashTable，所以关键点在于找出同字母异位词的唯一键。

1. 排序键。时间 ：
   $$
   O(N*KlogK)
   $$
   其中K是strs中最长字符串的长度。

   空间：
   $$
   O(N*K)
   $$
   

   ```java
   class Solution {
       public List<List<String>> groupAnagrams(String[] strs) {
           Map<String, List<String>> map = new HashMap<>();
           for(String str : strs) {
               char[] chArr = str.toCharArray();
               Arrays.sort(chArr);
               String key = String.valueOf(chArr);
               if(!map.containsKey(key)) {
                   map.put(key, new LinkedList<>());
               }
               map.get(key).add(str);
           }
           return new ArrayList(map.values());
       }
   }
   ```

   

2. 以字母出现次数为键。时间 ：
   $$
   O(N*K)
   $$
      其中K是strs中最长字符串的长度。

      空间：
   $$
   O(N*K)
   $$

   ```java
   public List<List<String>> groupAnagrams(String[] strs) {
       Map<String, List<String>> map = new HashMap<>();
       for(String str : strs) {
           int[] count = new int[26];
           for(char ch : str.toCharArray()) {
               count[ch - 'a']++;
           }
           //generate key
           StringBuilder sb = new StringBuilder();
           for(int i = 0; i < 26; i++) {
               sb.append('#');
               sb.append(count[i]);
           }
           String key = sb.toString();
           if(!map.containsKey(key)) {
               map.put(key, new LinkedList());
           }
           map.get(key).add(str);
       }
       return new ArrayList(map.values());
   }
   ```