# [157. 用 Read4 读取 N 个字符](https://leetcode-cn.com/problems/read-n-characters-given-read4/)

```java
/**
 * The read4 API is defined in the parent class Reader4.
 *     int read4(char[] buf);
 */
public class Solution extends Reader4 {
    /**
     * @param buf Destination buffer
     * @param n   Number of characters to read
     * @return    The number of actual characters read
     */
    public int read(char[] buf, int n) {
        int res = 0;
        int len = 0;
        char[] tempBuf = new char[4];
        //n代表剩余需要读的字符数
        //Math.min()防止多读
        //判断read4() != 0防止文件到达末尾
        while(n > 0 && (len = read4(tempBuf)) != 0) {
            System.arraycopy(tempBuf, 0, buf, res, Math.min(len, n));
            res += Math.min(len, n);
            n -= len;
        }
        return res;
    }
}
```

