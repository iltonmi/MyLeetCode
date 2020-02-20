#  150. Evaluate Reverse Polish Notation 

特型算法，emmm

1. 从前往后遍历，操作数直接入栈，遇到操作符弹出栈顶2个操作数。

   ```java
   public int evalRPN(String[] tokens) {
       Deque<Integer> q = new LinkedList<>();
       for(String token : tokens) {
           if(!isOperation(token)) {
               q.push(Integer.valueOf(token));
           } else {
               int divisor = q.pop();
               int dividend = q.pop();
               q.push(calculate(dividend, divisor, token));
           }
       }
       return q.peekFirst();
   }
   
   private boolean isOperation(String str) {
       return "+".equals(str) || "-".equals(str) || "*".equals(str) || "/".equals(str);
   }
   
   private Integer calculate(Integer a, Integer b, String op) {
       switch(op) {
           case "+":
               return a + b;
           case "-":
               return a - b;
           case "*":
               return a * b;
           case "/":
               return a / b;
           default:
               return 0;
       }
   }
   ```

   

   