#  70. Climbing Stairs

和斐波那契数列的文章提到的解法一样，这里只给出最优解。

```java
public int climbStairs(int n) {
    if(n < 0) {
        return 0;
    }
    if(n == 1 || n == 2) {
        return n;
    }
    int prepre = 1;
    int pre = 2;
    int res = 0;
    n -= 2;
    while(n-- > 0) {
        res =  pre + prepre;
        prepre = pre;
        pre = res;
    }
    return res;
}
```

