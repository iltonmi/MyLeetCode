# 155. Min Stack

核心思路是2个栈，一个常规栈，另一个最小值栈。

空间O(N)，所有操作时间O(1)。

1.  最小值栈只压入比小于或或等于栈顶的值；最小值栈只弹出和当前值一样的值。

   压入较为节省空间，弹出较为消耗时间。

   ```java
   class MinStack {
       Deque<Integer> stack, min ;
       public MinStack() {
           min = new ArrayDeque<Integer>();
           stack = new ArrayDeque<Integer>();
       }
       
       public void push(int x) {
           stack.push(x);
           if(min.isEmpty()) {
               min.push(x);
           } else if(x <= min.peek()){
               min.push(x);
           }
       }
       
       p。ublic void pop() {
           int x = stack.pop();
           if(!min.isEmpty() && x == min.peek()){
               min.pop();
           }
       }
       
       public int top() {
           return stack.peek();
       }
       
       public int getMin() {
           return min.peek();
       }
   }
   ```

   

2. 最小值栈，若小于栈顶的值，直接压入，否则重复压入栈顶值；最小值栈直接弹出。

   压入较为消耗空间，弹出较为节省时间。

   ```java
   class MinStack {
       Deque<Integer> stack, min ;
       public MinStack() {
           min = new ArrayDeque<Integer>();
           stack = new ArrayDeque<Integer>();
       }
       
       public void push(int x) {
           stack.push(x);
           if(min.peek() == null || x < min.peek()){
               min.push(x);
           } else {
               min.push(min.peek());
           }
       }
       
       public void pop() {
           stack.pop();
           min.pop();
       }
       
       public int top() {
           return stack.peek();
       }
       
       public int getMin() {
           return min.peek();
       }
   }
   ```

   