#  58. Length of Last Word 

1. 常规操作

   ```java
   public int lengthOfLastWord(String s) {
       if(s == null || s.length() == 0) {
           return 0;
       }
       int res = 0;
       int cur = 0;
       for(int i = 0; i < s.length(); i++) {
           if(s.charAt(i) == ' ') {
               cur = 0;
           } else {
               res = ++cur;
           }
       }
       return res;
   }
   ```

   

2. 违规操作

   ```java
   public int lengthOfLastWord(String s) {
       String a[] = s.split(" ");
   
       if(a.length == 0) {
           return 0;
       }
           
   
       return a[a.length-1].length();
   }
   ```

   