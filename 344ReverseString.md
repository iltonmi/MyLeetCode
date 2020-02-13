#  344. Reverse String

1. 双指针，空间O(1)，时间O(N)。

   ```java
   public void reverseString(char[] s) {
       if(s == null || s.length == 0) {
           return;
       }
       int lo = 0;
       int hi = s.length - 1;
       while(lo < hi) {
           char temp = s[lo];
           s[lo++] = s[hi];
           s[hi--] = temp;
       }
   }
   ```

   

2. 递归（闲得dan疼），空间O(N)，时间O(N)。

    https://leetcode.com/articles/reverse-string/ 

   ```java
   public void helper(char[] s, int left, int right) {
       if (left >= right) return;
       char tmp = s[left];
       s[left++] = s[right];
       s[right--] = tmp;
       helper(s, left, right);
   }
   
   public void reverseString(char[] s) {
       helper(s, 0, s.length - 1);
   }
   ```

   