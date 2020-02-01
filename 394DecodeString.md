#  394. Decode String

肯定要用栈，区别是用的递归还是迭代

1. 递归

   ```java
   class Solution {
       public String decodeString(String s) {
           Deque<Character> q = new LinkedList<>();
           for(char c : s.toCharArray()) {
               q.offerLast(c);
           }
           return helper(q);
       }
       
       private String helper(Deque<Character> q) {
           StringBuilder sb = new StringBuilder();
           int repeatTimes = 0;
           while(!q.isEmpty()) {
               char c = q.pollFirst();
               if(Character.isDigit(c)) {
                   repeatTimes = 10 * repeatTimes + c - '0';
               } else if(c == '[') {
                   //返回时，此'['对应的']'已经被消除
                   String temp = helper(q);
                   for(int i = 0; i < repeatTimes; i++) {
                       sb.append(temp);
                   }
                   repeatTimes = 0;
               } else if(c == ']') {
                   //终止条件，当前子表达式处理完成
                   break;
               } else {
                   sb.append(c);
               }
           }
           return sb.toString();
       }
   }
   ```

   

2. 迭代，从递归形式可知，除了使用队列显式保存的子表达式，还有函数栈局部变量表保存的重复次数和对应的子表达式。

   ​	这意味着，迭代时需要额外的2个栈，分别保存整型操作次数和字符串子表达式。

   ```java
   class Solution {
       public String decodeString(String s) {
           Deque<Integer> ints = new LinkedList<>();
           Deque<StringBuilder> strs = new LinkedList<>();
           //从递归形式可以知道，pre代表当前repeatTimes所对应的表达式的
           //前面已经解析过的字符串
           StringBuilder pre = new StringBuilder();
           int repeatTimes = 0;
           for(int i = 0; i < s.length(); i++) {
               char c = s.charAt(i);
               if(Character.isDigit(c)) {
                   repeatTimes = 10 * repeatTimes + (c - '0');
               } else if(c == '[') {
                   ints.push(repeatTimes);
                   strs.push(pre);
                   pre = new StringBuilder();
                   repeatTimes = 0;
               } else if(c == ']') {
                   StringBuilder sub = pre;
                   //子表达式前面的已经解析过的字符串
                   pre = strs.pop();
                   //子表达式sub对应的次数
                   //循环结束后，repeatTimes被重置为0
                   for(repeatTimes = ints.pop(); repeatTimes > 0; repeatTimes--) {
                       pre.append(sub);
                   }
               } else {
                   pre.append(c);
               }
           }
           return pre.toString();
       }
   }
   ```

   