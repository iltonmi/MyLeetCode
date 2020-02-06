#  66. Plus One

1. 因为这道题只加1，所以是否需要进位只取决于被加1的位置是不是9。
2. 检查每个需要加1的位置：如果需要进位，当前位置的9会变成0；否则结束。
3. 被进位的位置若不是9，直接加1并结束。
4. 结束后：若最高位不是0，说明长度不变；否则，最高位前增加一个值为1的位置。

空间O(1)，时间O(N)，代码如下：

```java
public int[] plusOne(int[] digits) {
    int i = digits.length - 1;
    for(; i >= 0; i--) {
        if(digits[i] == 9) {
            digits[i] = 0;
        } else {
            digits[i]++;
            break;
        }
    }
    if(digits[0] != 0) {
        return digits;
    } else {
        int[] res = new int[digits.length+1];
        res[0] = 1;
        System.arraycopy(digits, 0, res, 1, digits.length);
        return res;
    }
}
```

