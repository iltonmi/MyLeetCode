#  389. Find the Difference 

```java
public char findTheDifference(String s, String t) {
    int n = t.length();
    char c = t.charAt(n - 1);
    for (int i = 0; i < n - 1; ++i) {
        c ^= s.charAt(i);
        c ^= t.charAt(i);
    }
    return c;
}
```

