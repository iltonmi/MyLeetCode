#  172. Factorial Trailing Zeroes

 https://leetcode.com/problems/factorial-trailing-zeroes/discuss/?currentPage=1&orderBy=most_votes&query= 

> ```java
> 初步看到这道题，我首先是懵逼的，阶乘结果有多少个0？what??
> 硬来肯定是不行的，仔细一想，会出现零只有一种情况嘛：2*5=10;
> 所以我们可以将阶乘进行因式分解，比如5！=1*2*3*4*5=1*2*3*2*2*5,共有1对2*5，所以结果就只有一个0嘛;有几对2*5,结果就有几个0;
> 所以问题就简化为找有几对2*5,能不能进一步简化捏？必须可以啊!
> 我们发现2的个数是远远多于5的，所以最终简化为了找出5的个数！
> 如果找出5的个数呢，比如20!，从1到20,共有5，10，15,20,共有4个5，即结尾0的个数为n/5！
> 这样就ok么？当然没ok，25!结果不只是25/5=5个0，25结果有6个0,因为25=5*5，有两个5。所以结果f(n)=n/5+f(n/5)
> ```

```java
// 非递归解法，空间O(logN)，时间O(logN)
public static int trailingZeroes(int n) {
    if (n<5) {
        return 0;
    }
    return (n/5+ trailingZeroes(n/5));
}

// 递归解法，空间O(1)，时间O(logN)
public int trailingZeroes(int n) {
    int res = 0;
    while(n > 4) {
        n /= 5;
        res += n;         
    }
    return res;
}
```

