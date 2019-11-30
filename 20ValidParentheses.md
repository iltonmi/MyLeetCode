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
                stk.push(paren);
                continue;
            }
            char pop;
            if(!stk.isEmpty()) {
                pop = stk.pop();
            } else {
                return false;
            }
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