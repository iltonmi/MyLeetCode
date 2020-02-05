# 20. Valid Parentheses

1. 栈，时间O(n), 空间O(n)
```java
class Solution {
    public boolean isValid(String s) {
        char[] arr = s.toCharArray();
        Stack<Character> stk = new Stack<>();
        for(int i = 0; i < arr.length; i++) {
            char paren  = arr[i];
            if(paren == '(' || paren == '{' || paren == '[') {
                //左括号直接进栈
                stk.push(paren);
                continue;
            }
            char pop;
            if(!stk.isEmpty()) {
                //弹出类型不确定的左括号
                pop = stk.pop();
            } else {
                //右括号没有对应的左括号
                return false;
            }
            //检查左括号是否属于对应的类型
            if(paren == ')' && pop != '(') {
                return false;
            }
            if(paren == '}' && pop != '{') {
                return false;
            }
            if(paren == ']' && pop != '[') {
                return false;
            }
        }
        return stk.isEmpty();
    }
}
```