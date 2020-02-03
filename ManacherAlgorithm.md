# Manacher's Algorithm

## 时间复杂度O(n)分析

1. 首先，我们从代码中发现，关键在于嵌套循环。
2. 对于马拉车算法，并不是所有情况下都能利用p数组进行加速。
3. 当无法利用p数组进行加速的情况下，意味着需要对所有已找到的回文串中最右的边界继续进行右扩。
4. 这一过程在如下代码中的体现是：进入内循环。
5. 明显，最大右扩的次数 = manacher string的长度值，即2n+1次。
6. 这就是内循环执行的最大次数，均摊到外循环，相当于O(1)。
7. 而外循环执行的次数也是2n+1，所以复杂度为O(n)。

```java
public static int maxLcpsLength(String str) {
    if (str == null || str.length() == 0) {
        return 0;
    }
    char[] charArr = manacherString(str);
    //i位置上字符作为回文中心的最大回文半径
    int[] pArr = new int[charArr.length];
    //最近一次更新r时，回文中心的位置
    int index = -1;
    //之前遍历的所有字符的回文半径，最右即将到达的位置，即最右的回文的下一个位置。
    int r = -1;
    int max = Integer.MIN_VALUE;
    for (int i = 0; i != charArr.length; i++) {
        //r与i的情况分为2大类，第2个大类分为3种情况，总共4种情况
        //1. r <= i
        //2. r > i：
        //2.1：i + pArr[i] - 1 = r - 1，即分别以i和index为中心的两个最大回文串右边界重合。
        //2.2、2.3：i + pArr[i] - 1 < r - 1, i + pArr[i]  - 1 > r - 1
        pArr[i] = r > i ? Math.min(pArr[2 * index - i], r - i) : 1;
        //到此为止，对于2.2和2.3的情况，pArr[i]已经确定，
        //即：charArr[i - pArr[i]] ！= charArr[i + pArr[i]]
        //所以2.2和2.3不可能进入循环
        while (i + pArr[i] < charArr.length && i - pArr[i] > -1 &&
              charArr[i + pArr[i]] == charArr[i - pArr[i]]
              ) {
            pArr[i]++
        }
        if (i + pArr[i] > r) {
            r = i + pArr[i];
            index = i;
        }
        max = Math.max(max, pArr[i]);
    }
    return max - 1;
}

public static String manacherString(String str) {
    char[] charArr = str.toCharArray();
    //无论奇数偶数，想象一下如下情景：
    //n个人排队，每个人的前面插入新来的人，队伍就会有2n个人
    //再有一个人从队尾进入队列。到此为止，队伍有2n+1个人
    char[] res = new char[str.length() * 2 + 1];
    int index = 0;
    for (int i = 0; i != res.length; i++) {
        res[i] = (i & 1) == 0 ? '#' : charArr[index++];
    }
    return new String(res);
}
```

## 应用

从LeetCode的一些解答发现，其实不需要真正生成manacher string。

1. 关键在于p数组中，对于索引 i ，只要满足 (i & 1) == 0，则此索引对应manacher string中的字符就是分隔符。
2. 只要输入字符串不为空，无需关心manacher string中回文串的轴是普通字符还是分隔符，最后答案对应manacher string的边界符必然都是分隔符。举个例子，“#a#”很显然只有两种回文，“#“和“#a#”本身。
3. 从manacher string中非分隔符到original string的索引映射为：i -> i >> 1。