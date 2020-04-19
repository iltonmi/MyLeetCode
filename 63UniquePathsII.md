#  63. Unique Paths II 

dp基本操作，基本条件 + 状态转移

```java
public int uniquePathsWithObstacles(int[][] obstacleGrid) {
    //到达目标地点的路径数 = 目标地点有障碍 ? 0 : 到达目标地点上方的路径数 + 到达目标地点左方的路径数;
    int r = obstacleGrid.length;
    int c = obstacleGrid[0].length;
    int[][] dp = new int[r][c];
    if (obstacleGrid[0][0] == 1) {
        return 0;
    }
    dp[0][0] = 1;
    for(int i = 1; i < r; i++) {
        dp[i][0] = (dp[i-1][0] == 0 || obstacleGrid[i][0] == 1) ? 0 : 1;
    }
    for(int i = 1; i < c; i++) {
        dp[0][i] = (dp[0][i-1] == 0 || obstacleGrid[0][i] == 1) ? 0 : 1;
    }
    for(int i = 1; i < r; i++) {
        for(int j = 1; j < c; j++) {
            dp[i][j] = obstacleGrid[i][j] == 0 ? dp[i-1][j] + dp[i][j-1] : 0;
        }
    }
    return dp[r - 1][c - 1];
}
```

