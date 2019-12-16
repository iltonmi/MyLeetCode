# 48. Rotate Image

这是一个很规则的正方形矩阵。

1. 旋转每一圈，时间O(N^2)，空间O(1)

   ```java
   public void rotate(int[][] matrix) {
       int len = matrix.length;
       //i是每一圈第一行的索引
       for(int i = 0; i <= len / 2 - 1; i++) {
           //每一圈的第一行的最后一个元素的索引
           int end = len-i-1;
           //j是每一圈第i行的元素的列索引
           for(int j = i; j < end; j++) {
               int temp = matrix[i][j];
               matrix[i][j] = matrix[len-j-1][i];
               matrix[len-j-1][i] = matrix[end][len-j-1];
               matrix[end][len-j-1] = matrix[j][end];
               matrix[j][end] = temp;
           }
       }
   }
   ```

   