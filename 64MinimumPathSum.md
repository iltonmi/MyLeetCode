#  64. Minimum Path Sum

状态转移方程很直观。
$$
f(m, n)= nums[m][n] + min(f(m-1, n), f(m, n-1))
$$
因为只能向右或者向下扩展，因此：
$$
f(1, k) = f(1, k-1) + nums[1][k]
$$

$$
f(k, 1) = f(k-1, 1) + nums[k][1]
$$

1. 修改输入值，时间O(MN)，空间O(1)

   ```java
   public int minPathSum(int[][] grid) {
       int row = grid.length;
       int col = grid[0].length;
       //initialization
       for(int i = 1; i < row; i++) {
           grid[i][0] += grid[i-1][0];
       }
       //initialization
       for(int i = 1; i < col; i++) {
           grid[0][i] += grid[0][i-1];
       }
       if(row == 1 || col == 1) {
           return grid[row - 1][col - 1];
       }
       for(int i = 1; i < row; i++) {
           for(int j = 1; j < col; j++) {
               grid[i][j] += Math.min(grid[i-1][j], grid[i][j-1]);
           }
       }
       return grid[row - 1][col - 1];
   }
   ```

   

2. 不修改输入值，时间O(MN)，空间O(min(M, N))

   ```java
   public int minPathSum(int[][] grid) {
       int row = grid.length;
       int col = grid[0].length;
       int[] dp = new int[row < col ? row : col];
       return row < col ? rowSmaller(grid, dp, row, col) 
           : colSmaller(grid, dp, row, col);
   }
   
   private int rowSmaller(int[][] grid, int[] dp, int row, int col) {
       dp[0] = grid[0][0];
       for(int i = 1; i < dp.length; i++) {
           dp[i] = grid[i][0] + dp[i-1];
       }
       for(int j = 1; j < col; j++) {
           //这个容易漏掉，grid[0][j]上方没有数，需要特别处理
           dp[0] += grid[0][j];
           for(int i = 1; i < row; i++) {
               dp[i] = grid[i][j] + Math.min(dp[i], dp[i-1]);
           }
       }
       return dp[row-1];
   }
   
   private int colSmaller(int[][] grid, int[] dp, int row, int col) {
       dp[0] = grid[0][0];
       for(int i = 1; i < dp.length; i++) {
           dp[i] = grid[0][i] + dp[i-1];
       }
       for(int i = 1; i < row; i++) {
           //这个容易漏掉，grid[i][0]左边没有数，需要特别处理
           dp[0] += grid[i][0];
           for(int j = 1; j < col; j++) {
               dp[j] = grid[i][j] + Math.min(dp[j-1], dp[j]);
           }
       }
       return dp[col-1];
   }
   ```

   

3. 占位