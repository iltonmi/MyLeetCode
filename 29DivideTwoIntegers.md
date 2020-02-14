#  29. Divide Two Integers

原理请看另一篇文章：只用位运算实现加减乘除。

```java
public int divide(int a, int b) {
    if(a == Integer.MIN_VALUE && b == Integer.MIN_VALUE) {
        return 1;
    } else if(b == Integer.MIN_VALUE) {
        return 0;
    } else if(a == Integer.MIN_VALUE) {
        //题目规定溢出返回MAX_VALUE
        if(b == -1) {
            return Integer.MAX_VALUE;
        }
        int temp = div(a + 1, b);
        return temp + div(a - multi(temp, b), b);
    } else {
        return div(a, b);
    }
}

int div(int a, int b) {
    int x = isNeg(a) ? -a : a;
    int y = isNeg(b) ? -b : b;
    int res = 0;
    for(int i = 31; i > -1; i--) {
        if((x >> i) >= y) {
            res |= (1 << i);
            x = x - (y << i);
        }
    }
    return isNeg(a) ^ isNeg(b) ? -res : res;
}

public int multi(int a, int b) {
    int res = 0;
    while(b != 0) {
        if((b & 1) != 0) {
            res += a;
        }
        a <<= 1;
        b >>>= 1;
    }
    return res;
}

boolean isNeg(int n) {
    return n < 0;
}
```

