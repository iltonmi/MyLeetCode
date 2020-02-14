#  8. String to Integer (atoi)

1. 处理空格
2. 处理符号位
3. 若第一个位置是数字则进入循环，开始计算
4. 处理溢出

无视符号的情况下，检查当前res。

1. 若res溢出，则判断符号，返回最值。

分析：

1. 符号是正，若res溢出，返回最大值。
2. 符号位负，若res溢出，说明最后的答案至少是Integer.MIN_VALUE，按照题目规定返回最小值。

```java
public int myAtoi(String str) {
    char sign = '\0';
    int res = 0;
    int i = 0;
    //处理空格
    for(; i < str.length() && str.charAt(i) == ' '; i++) {}
    //处理符号位
    if(i < str.length() && (str.charAt(i) == '+' || str.charAt(i) == '-')) {
        sign = str.charAt(i);
        i++;
    }
    //若开头是数字则循环
    for(; i < str.length(); i++) {
        char ch = str.charAt(i);
        if(!isDigit(ch)) {
            break;
        }
        int tail = ch - '0';
        //处理溢出
        if(res > Integer.MAX_VALUE / 10
           || (res == Integer.MAX_VALUE / 10 && tail > Integer.MAX_VALUE % 10)) {
            if(sign == '-') {
                return Integer.MIN_VALUE;
            } else {
                return Integer.MAX_VALUE;
            }
        }
        res = res * 10 + tail;
    }
    return sign != '-' ? res : -res;
}

//记住加等号（手动狗头）
private boolean isDigit(char ch) {
    return ch - '0' < 10 && ch - '0' >= 0;
}
```

