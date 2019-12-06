#  17. Letter Combinations of a Phone Number 

1. 回溯，但由于全是正确解，退化为递归。时间O(n^2)，空间O(n)。

   1. [Backtracking](https://en.wikipedia.org/wiki/Backtracking) is an algorithm for finding all solutions by exploring all potential candidates. If the solution candidate turns to be *not* a solution (or at least not the *last* one), backtracking algorithm discards it by making some changes on the previous step, *i.e.* *backtracks* and then try again. 

   2. Time complexity : 

   $$
   \mathcal{O}(3^N \times 4^M)
   $$

   where `N` is the number of digits in the input that maps to 3 letters (*e.g.* `2, 3, 4, 5, 6, 8`) and `M` is the number of digits in the input that maps to 4 letters (*e.g.* `7, 9`), and `N+M` is the total number digits in the input. 

   3.  Space complexity : 
      $$
      如果算上返回值：\mathcal{O}(3^N \times 4^M);否则是递归栈深度：O(N)
      $$

   ```java
   class Solution {
       private String[] map = new String[]{null, null, "abc", "def", "ghi", "jkl", "mno",
                                          "pqrs", "tuv", "wxyz"};
       private String digits;
       private int val;
       private List<String> res = new LinkedList<>();
       public List<String> letterCombinations(String digits) {
           if(digits == null || digits.length() == 0) {
               return res;
           }
           this.digits = digits;
           this.val = Integer.parseInt(digits);
           help("", 0);
           return res;
       }
       
       private void help(String prefix, int from) {
           if(from == digits.length()) {
               res.add(prefix);
               return;
           }
           String str = map[ ((int) (val /
           Math.pow(10, digits.length() - (from + 1)))) % 10];
           for(int i = 0; i < str.length(); i++) {
               // StringBuilder s = new StringBuilder(prefix);
               // s.append(str.charAt(i));
               // help(s.toString(), from+1);
               help(prefix + Character.toString(str.charAt(i)), from + 1);
           }
       }
   }
   ```