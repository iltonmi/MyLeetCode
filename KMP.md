# KMP

> 从 [stackoverflow](https://stackoverflow.com/questions/9182651/whats-the-worst-case-complexity-for-kmp-when-the-goal-is-to-find-all-occurrence?r=SearchResults)中找到了一个时间复杂度分析很棒的链接 https://www.inf.hs-flensburg.de/lang/algorithmen/pattern/kmpen.htm 

判断字符串 str 中是否包含子串 match。 

**next [i]** : match [i-1] 结尾的后缀子串（不包含match [0]）和 match [0] 开头的前缀子串，两者的最大匹配长度。

1. 因为match[0] 前面没有字符串，规定 next [0] == -1
2. 因为 next [i] 对应的子串不包含 match [0]，所以next[1] = 0

**假设当前 str[i ... j-1] 和 match [0 ... j-i-1]：若 str [j] != match [j-i]**，

1. 若next [j] != -1，下一个比较的位置不是 str [i+1] 和 match [0]，而是 str[j] 和 match [next [j-i]]
2. 若next [j] = -1，说明 match 的索引指向 match [0]，即 j - i = 0，并且在上一次比较中，match [0] != str [j]，此时 str 的索引加1即可。

算法的精髓在于**搞清楚一个问题：str [i] 和 str [j] 之间是否存在以 str [j-1] 结尾且长度大于 next[j-i] 的子串呢？**

答案显然是否定的，这**违反了 next 数组的定义。**

时间复杂度：O(N)，分析：

**先看**匹配过程：

1. 方法的循环体中有3个分支。
2. 循环中，si++发生的次数等于 s.length - 1。因此，进入前2个分支的次数是 s.length - 1。
3. 其次，mi回退(match滑动)的过程等价于 match 对应 str 往右至少一个位置，显然它往右（match滑动）的最大次数是 s.length - m.length。因此，进入最后1个分支的次数是s.length - m.length。
4. 所以循环发生的次数 2 * s.length - m.length + 1，即2N-M+1。

**再看**next数组生成：

1. 方法的循环体中有3个分支。
2. 循环中，pos++发生的次数等于 m.length - 2。因此，进其中2个分支的次数是 m.length - 2。
3. 其次，cn回退最多发生多少次，受限制于 ++cn 执行了多少次，++cn 和 pos++ 同时发生最多发生的次数是 m.length - 2。
4. 所以循环发生的次数 2 * m.length - 4，即2M-4。

**最后看**总复杂度：

1. (2N-M+1) + (2M-4) = (2N+M-3) = O(2N+M)
2. 因为 N >= M，O(2N+M) = O(3N) = O(N)

```java
public static int getIndexOf(String s, String m) {
    if (s == null || m == null || m.length() < 1 || s.length() < m.length()) {
        return -1;
    }
    char[] ss = s.toCharArray();
    char[] ms = m.toCharArray();
    int si = 0;
    int mi = 0;
    int[] next = getNextArray(ms);
    while (si < ss.length && mi < ms.length) {
        if (ss[si] == ms[mi]) {
            //匹配
            si++;
            mi++; 
        } else if (next[mi] == -1) {
            //当前mi = 0，str[si] != match[0]，si++即可
            si++;
        } else {
            //滑动
            mi = next[mi];
        }
    }
    return mi == ms.length ? si - mi : -1;
}
```

**怎么计算next数组？**

1. match [0] == -1，match[1] = 0（原因已经给出）。
2. 从左至右依次计算，计算 next [i] 时已知 next [0 ... i-1]
3. 我们可以利用 next [i - 1]，若 match [i-1] = match [next [i-1]]，那么 next [i] = next[i-1] + 1(再长的话与next数组定义违背)
4. 若 match [i-1] ！= match [next [i-1]]，则比较 match[i-1] 和 match[next[next[i-1]]]，原因如下：
   1. 假设next[i-1] 对应前后缀分别是A和B，那么 next [next [i-1]] 则代表A的前后缀最大匹配长度。
   2. 由于A=B，因此A的前缀能对应B的后缀。
   3. 当前可能性不可能大于 next [next [i-1]] + 1，否则与next数组定义违背。
5. 第3步和第4步递归执行，直到 next [k] = 0，则令 next [i] = 0。

```java
public static int[] getNextArray(char[] ms) {
    if (ms.length == 1) {
        return new int[] { -1 };
    }
    int[] next = new int[ms.length];
    next[0] = -1;
    next[1] = 0;
    //当前将要计算的位置
    int pos = 2;
    //当前将要被比较的位置
    int cn = 0;
    while (pos < next.length) { 
        if (ms[pos - 1] == ms[cn]) {
            next[pos++] = ++cn;
            //此刻，cn = next[pos - 1]
        } else if (cn > 0) { 
            cn = next[cn];
        } else {
            next[pos++] = 0;
            //此刻，cn = next[pos-1] = 0
        }
    }
    return next;
}
```

