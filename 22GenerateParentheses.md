#  22. Generate Parentheses

1. 递归枚举，重点：

   1. 如何枚举
   2. 如何验证字符串是否符合要求：为了检查序列是否为有效的，我们会跟踪 `平衡`，也就是左括号的数量减去右括号的数量的净值。如果这个值始终小于零或者不以零结束，该序列就是无效的，否则它是有效的。 

   3. 时间、空间：

   $$
   O(2^{2n}),若不算解占用的空间，空间复杂度应为递归栈最大深度O(n)
   $$

   ```java
   class Solution {
       List<String> res = new LinkedList<>();
       public List<String> generateParenthesis(int n) {
           generateAll(new char[2 * n], 0);
           return res;
       }
   
       public void generateAll(char[] current, int pos) {
           if (pos == current.length) {
               if (valid(current))
                   res.add(new String(current));
           } else {
               current[pos] = '(';
               generateAll(current, pos+1);
               current[pos] = ')';
               generateAll(current, pos+1);
           }
       }
   
       public boolean valid(char[] current) {
           int balance = 0;
           for (char c: current) {
               if (c == '(') {
                   balance++;
               } else {
                   balance--;
               }
               if (balance < 0 || balance > current.length >> 1) {
                   return false;
               }
           }
           return balance == 0;
       }
   }
   ```

2. 回溯:

   记录到目前为止放置的左括号和右括号的数目，若不符合要求则马上停止递归。

   

3. 只有在我们知道序列仍然保持有效时才添加 '(' or ')'，而不是像 方法一 那样每次添加。我们可以通过跟踪到目前为止放置的左括号和右括号的数目来做到这一点，

   如果我们还剩一个位置，我们可以开始放一个左括号。 如果它不超过左括号的数量，我们可以放一个右括号。
   
