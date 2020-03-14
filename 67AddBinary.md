#  67. Add Binary 

经典。

```java
public String addBinary(String a, String b) {
    //长
    int n = a.length();
    //短
    int m = b.length();
    if (n < m) {
        return addBinary(b, a);
    }
    int L = Math.max(n, m);

    StringBuilder sb = new StringBuilder();
    int carry = 0;
    int j = m - 1;
    for(int i = L - 1; i > -1; --i) {
      if (a.charAt(i) == '1') {
          ++carry;
      }
      if (j > -1 && b.charAt(j--) == '1') {
          ++carry;
      }

      if (carry % 2 == 1) {
          sb.append('1');
      } else {
          sb.append('0');
      }

      carry /= 2;
    }
    if (carry == 1) {
        sb.append('1');
    }
    sb.reverse();
    return sb.toString();
}
```

