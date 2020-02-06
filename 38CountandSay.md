#  38. Count and Say

纯属找规律。

来看下面的前十个序列：

```java
 1.     1
 2.     11
 3.     21
 4.     1211
 5.     111221 
 6.     312211
 7.     13112221
 8.     1113213211
 9.     31131211131221
10.     13211311123113112211
```

**以第6个序列举例：**

1. 已知序列5： 111221
2. 连续遇到3个1，即“31”。
3. 连续遇到2个2，即“22”。
4. 遇到单独的1，即“11”。
5. 组合起来得到序列6：“312211”。

**3个规律：**

1. 第一个序列为1。
2. 每个序列都是对前一个序列的描述。
3. 描述规则：从左到右，连续遇到N个X，则插入“MX”；若遇到单独的X（即X的左右都不是X），则插入“1X”。

时间O(N)，空间O(1)代码如下。

```java
public String countAndSay(int n) {
    String res = "1";
    if(n == 1) {
        return res;
    }
    for(int i = 2; i <= n; i++) {
        StringBuilder sb = new StringBuilder();
        char pre = '*';
        int count = 0;
        for(char cur : res.toCharArray()) {
            if(pre != cur) {
                if(count > 0) {
                    sb.append(count);
                    sb.append(pre);
                }
                pre = cur;
                count = 1;
            } else {
                count++;
            }
        }
        sb.append(count);
        sb.append(pre);
        res = sb.toString();
    }
    return res;
}
```

