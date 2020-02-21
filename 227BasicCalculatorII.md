#  227. Basic Calculator II

基于栈的思想：

1. 若栈顶元素不是 “*” 或 “/”，直接入栈。
2. 否则，连续弹出2个元素。第一个弹出操作符，第二个弹出操作数，将弹出的操作数和当前元素进行恰当的运算，将结果入栈。
3. 若是最后一个元素，依次弹出操作符和操作数，进行相应加减运算，然后将结果变为下一个操作数，重复此步。

1. 迭代。将减法看作对负数的加法，将乘除法的结果当作整体的值，就变成了加法运算。空间O(1)，时间O(N)。

   1. 对于乘除法，将当前值 curVal 和 preVal 进行相应的运算并保存到 preVal。
   2. 对于加减法，将preVal加到sum。将减法看作对负数的加法，因为不知道下一个操作符的优先级，所以将值保存到preVal。

   ```java
   public int calculate(String s) {
       int sum = 0;
       int preVal = 0;
       int curVal = 0;
       char lastSign = '+';
       for (int i = 0; i < s.length(); i++) {
           char ch = s.charAt(i);
           if (Character.isDigit(ch)) {
               curVal = curVal * 10 + ch - '0';
           }
           if (i == s.length() - 1 || (!Character.isDigit(ch) && ch != ' ')) {
               switch(lastSign) {
                   case '+':
                       sum += preVal;
                       preVal = curVal;
                       break;
                   case '-':
                       sum += preVal;
                       preVal = -curVal;
                       break;
                   case '*':
                       preVal *= curVal;
                       break;
                   case '/':
                       preVal /= curVal;
                       break;
               }
               lastSign = ch;
               curVal = 0;
           }
       }
       sum += preVal;
       return sum;
   }
   ```

   