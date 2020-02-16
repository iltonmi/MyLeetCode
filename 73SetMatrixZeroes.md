# 73. Set Matrix Zeroes

 https://leetcode.com/articles/set-matrix-zeroes/ 

1. 不能未卜先知，至少扫描2次
2. 不能马上将相关行列设置为0，否则未检查的信息就丢失了
3. 不能马上将相关行列的非0值设置为哨兵值，因为可能会等于原来的值，这意味着存在一些值不应该被设置为0，但却和哨兵值相等。



1. 散列表标记哪一行需要被设置为0，第二次遍历只需查看左边是否在散列表中。

   ```java
   public void setZeroes(int[][] matrix) {
       int R = matrix.length;
       int C = matrix[0].length;
       Set<Integer> rows = new HashSet<Integer>();
       Set<Integer> cols = new HashSet<Integer>();
   
       // Essentially, we mark the rows and columns that are to be made zero
       for (int i = 0; i < R; i++) {
         for (int j = 0; j < C; j++) {
           if (matrix[i][j] == 0) {
             rows.add(i);
             cols.add(j);
           }
         }
       }
   
       // Iterate over the array once again and using the rows and cols sets, update the elements.
       for (int i = 0; i < R; i++) {
         for (int j = 0; j < C; j++) {
           if (rows.contains(i) || cols.contains(j)) {
             matrix[i][j] = 0;
           }
         }
       }
   }
   ```

   

2. 以第一行和第一列为标记。

   **注意1：** 若第一行或第一列本身不是0，这意味着，第一行和第一列的信息，可能丢失。

   因此我们选择按行遍历，即先遍历第一行。至于第一列，对于每行从左向右。这样第一行和第一列就可以用来标记，而且不用担心信息的丢失。

   **注意2：**第一行和第一列的标记是 (0, 0)，重叠了，所以可以还需要另一个变量，分别代表第一行和第一列列是否需要标记。

   ```java
   public void setZeroes(int[][] matrix) {
      	//使用isCol标记第一列，因此matrix[0][0]标记的是第一行
       Boolean isCol = false;
       int R = matrix.length;
       int C = matrix[0].length;
   
       for (int i = 0; i < R; i++) {
         // Since first cell for both first row and first column is the same i.e. matrix[0][0]
         // We can use an additional variable for either the first row/column.
         // For this solution we are using an additional variable for the first column
         // and using matrix[0][0] for the first row.
         if (matrix[i][0] == 0) {
             //若每行的第一个，即第一列的元素是0，设置isCol为true
           isCol = true;
         }
   
         for (int j = 1; j < C; j++) {
           // If an element is zero, we set the first element of the corresponding row and column to 0
           if (matrix[i][j] == 0) {
             matrix[0][j] = 0;
             matrix[i][0] = 0;
           }
         }
       }
   
       // Iterate over the array once again and using the first row and first column, update the elements.
       for (int i = 1; i < R; i++) {
         for (int j = 1; j < C; j++) {
           if (matrix[i][0] == 0 || matrix[0][j] == 0) {
             matrix[i][j] = 0;
           }
         }
       }
   
       // See if the first row needs to be set to zero as well
       if (matrix[0][0] == 0) {
         for (int j = 0; j < C; j++) {
           matrix[0][j] = 0;
         }
       }
   
       // See if the first column needs to be set to zero as well
       if (isCol) {
         for (int i = 0; i < R; i++) {
           matrix[i][0] = 0;
         }
       }
   }
   ```

   