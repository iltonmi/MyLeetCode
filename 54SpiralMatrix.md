#  54. Spiral Matrix

通用解法：定位左上角和右下角的左边。

本题注意：矩阵可能只有一列或者一行。

 空间O(1), 时间O(M * N)

```java
class Solution {
    
    private int[][] m;
    
    public List<Integer> spiralOrder(int[][] matrix) {
        if(matrix == null || matrix.length == 0 || matrix[0].length == 0) {
            return new ArrayList<>();
        }
        int row = matrix.length;
        int col = matrix[0].length;
        List<Integer> res = new ArrayList<>(row * col);
        int tr = 0;
        int tc = 0;
        int dr = matrix.length - 1;
        int dc = matrix[0].length - 1;
        this.m = matrix;
        while(tr <= dr && tc <= dc) {
            addEdge(res, tr++, tc++, dr--, dc--);
        }
        return res;
    }
    
    private void addEdge(List<Integer> res, int tr, int tc, int dr, int dc) {
        if(tr == dr) {
            //only one row
            for(int i = tc; i <= dc; i++) {
                res.add(m[tr][i]);
            }
        } else if(tc == dc) {
            //only one col
            for(int i = tr; i <= dr; i++) {
                res.add(m[i][tr]);
            }
        } else {
            //normal
            int r = tr;
            int c = tc;
            while(c != dc) {
                res.add(m[r][c++]);
            }
            while(r != dr) {
                res.add(m[r++][c]);
            }
            while(c != tc) {
                res.add(m[r][c--]);
            }
            while(r != tr) {
                res.add(m[r--][c]);
            }
        }
    }
}
```

