#  62. Unique Paths

1. DP，时间O(m*n)，空间O(min(m, n))。

   $$
   f(m, n) = f(m-1, n) + f(m, n-1)
   $$

   $$
   其中：f(1, 1...n) = 1; f(1...m, 1) = 1
   $$

   ```java
   public int uniquePaths(int m, int n) {
       if(m == 1 || n == 1) {
           return 1;
       }
       int[] dp = new int[Math.min(m, n)];
       Arrays.fill(dp, 1);
       for(int i = 0; i < Math.max(m, n) - 1; i++) {
           for(int j = 1; j < dp.length; j++) {
               dp[j] += dp[j-1];
           }
       }
       return dp[dp.length - 1];
   }
   ```

2. 枚举，时间O(m*n)，空间O(min(m, n))，用long避免溢出。

   随机排列向左，或向右：
   $$
   res = C_{m+n}^{min(m,n)}
   $$

   ```java
   public int uniquePaths(int m, int n) {
       if(m == 1 || n == 1) {
           return 1;
       }
       long res = 1;
       m--;
       n--;
       //let m be the bigger integer
       if(m < n) {
           int temp = m;
           m = n;
           n = temp;
       }
       for(int i = m + 1, j = 1; i <= m + n; i++, j++) {
           res *= i;
           res /= j;
       }
       return (int) res;
   }
   ```

3. 