#  74. Search a 2D Matrix 

1. 映射成一维数组，二分查找，空间O(1)，时间O(log(MN))

   ```java
   public boolean searchMatrix(int[][] matrix, int target) {
           int m, n;
       if(matrix == null 
          || (m = matrix.length) == 0 || (n = matrix[0].length) == 0) {
           return false;
       }
       int l = 0;
       int r =  m * n - 1;
       while(l <= r) {
           int mid = l + (r - l) / 2;
           int cur = matrix[mid / n][mid % n];
           if(target == cur) {
               return true;
           } else if(target > cur) {
               l = mid + 1;
           } else {
               r = mid - 1;
           }
       }
       return false;
   }
   ```

   

2. 排除法，空间O(1)，时间O(max(M, N))

   ```java
   public boolean searchMatrix(int[][] matrix, int target) {
       int r, c;
       if(matrix == null 
          || (r = matrix.length) == 0 || (c = matrix[0].length) == 0) {
           return false;
       }
       int i = r - 1;
       int j = 0;
       while(i > -1 && j < c) {
           if(matrix[i][j] == target) {
               return true;
           } else if(target > matrix[i][j]) {
               j++;
           } else {
               i--;
           }
       }
       return false;
   }
   ```

   