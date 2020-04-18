#  59. Spiral Matrix II 

通用解法见54. Spiral Matrix

```java
public int[][] generateMatrix(int n) {
    int[][] res = new int[n][n];
    int limit = n * n;
    int tr = 0, tc = 0, dr = res.length - 1, dc = res[0].length - 1;
    int val = 1;
    while(tr <= dr && tc <= dc) {
        if(tr == dr) {
        //only one row
        for(int i = tc; i <= dc; i++) {
            res[tr][i] = val++;
        }
    } else if(tc == dc) {
        //only one col
        for(int i = tr; i <= dr; i++) {
            res[i][tr] = val++;
        }
    } else {
        //normal
        int r = tr;
        int c = tc;
        while(c != dc) {
            res[r][c++] = val++;
        }
        while(r != dr) {
            res[r++][c] = val++;
        }
        while(c != tc) {
            res[r][c--] = val++;
        }
        while(r != tr) {
            res[r--][c] = val++;
        }
    }
        tr++; tc++; dr--; dc--;
    }
    return res;
}
```

