#  205. Isomorphic Strings 

1. 从映射的位置和被映射的位置分析

   ```java
   public boolean isIsomorphic(String s, String t) {
       if(s.length() != t.length()) {
           return false;
       }
       //对字符k进行映射位置
       int[] map1 = new int[256];
       //字符v被映射的位置
       int[] map2 = new int[256];
       for(int i = 0; i < s.length(); i++) {
           char a = s.charAt(i);
           char b = t.charAt(i);
           //限制：
           //1. 一个字符最多可以被一个字符映射，字符可以映射到自己本身
           //2. 所有字符都必须被替换！！！！！！！！！！！！！！
   
           //若map1[a] != map2[b]:
           //1. map1[a] < map2[b]，a没有映射到b；或a映射b后，b又被其他字符映射
           //2. map1[a] > map2[b]，a没有映射到b；或a映射b后，a映射了其他字符
           //若map1[a] == map2[b]: a映射到b；或a没有映射任何值，b也没有被任何值映射
           if(map1[a] != map2[b]) {
               return false;
           }
           //映射字符
           map1[a] = i + 1;
           map2[b] = i + 1;
       }
       return true;
   }
   ```

   

2. 从映射的字符和被映射的字符分析

   ```java
   public boolean isIsomorphic(String s, String t) {
       if(s.length() != t.length()) {
           return false;
       }
       //被字符k映射到的字符v
       int[] map1 = new int[257];
       //映射到字符v的字符k
       int[] map2 = new int[257];
       for(int i = 0; i < s.length(); i++) {
           int a = s.charAt(i) + 1;
           int b = t.charAt(i) + 1;
           //限制：
           //1. 一个字符最多可以被一个字符映射，字符可以映射到自己本身
           //2. 所有字符都必须被替换！！！！！！！！！！！！！！
           if(map1[a] != 0) {
               //a存在映射但不是b
               if(map1[a] != b) {
                   return false;
               }
           } else {
               //b已经被其他字符映射
               if(map2[b] != 0) {
                   return false;
               }
               map1[a] = b;
               map2[b] = a;
           }
       }
       return true;
   }
   ```

   