#  200. Number of Islands 

这题没什么好说的，就是硬菜。

显然可以运用深度优先搜索和广度优先搜索，还可以使用并查集。

1. 深度优先搜索，时间和空间O(M * N)

   ```java
       private char[][] grid;
       public int numIslands(char[][] grid) {
           if(grid == null || grid.length == 0) {
               return 0;
           }
           int res = 0;
           this.grid = grid;
           for(int i = 0; i < grid.length; i++) {
               for(int j = 0; j < grid[0].length; j++) {
                   if(grid[i][j] == '1') {
                       res++;
                       dfs(i, j);
                   }
               }
           }
           return res;
       }
       
       private void dfs(int i, int j) {
           if(i < 0 || j < 0 || i >= grid.length || j >= grid[0].length 
              || grid[i][j] != '1') {
               return;
           }
           
           grid[i][j] = '2';
           dfs(i - 1, j);
           dfs(i + 1, j);
           dfs(i, j - 1);
           dfs(i, j + 1);
       }
   }
   ```

   

2. 广度优先搜索，时间O(M * N)，空间O(Min(M, N))，在leetcode会超时。

   这里有一个值得学习的地方：如何**使用一个值表示矩阵中的坐标，这个值在矩阵中是唯一的**。

   ```java
   int val = ri * col + c;
   
   int r = val / col;
   
   int c = val % col;
   
   class Solution {
       public int numIslands(char[][] grid) {
           if(grid == null || grid.length == 0) {
               return 0;
           }
           int res = 0;
           Deque<Integer> q = new ArrayDeque<>(Math.max(grid.length, grid[0].length));
           int row = grid.length;
           int col = grid[0].length;
           for(int i = 0; i < row; i++) {
               for(int j = 0; j < col; j++) {
                   if(grid[i][j] == '1') {
                       ++res;
                       grid[i][j] = '2';
                       q.add(getId(i, j, col));
                       while(!q.isEmpty()) {
                           int id = q.poll();
                           int r = id / col;
                           int c = id % col;
                           if(r - 1 >= 0 && grid[r - 1][c] == '1') {
                               grid[r - 1][j] = '2';
                               q.add(getId(r - 1, c, col));
                           }
                           if(r + 1 < row && grid[r + 1][c] == '1') {
                               grid[r + 1][j] = '2';
                               q.add(getId(r + 1, c, col));
                           }
                           if(c - 1 >= 0 && grid[r][c - 1] == '1') {
                               grid[r][c - 1] = '2';
                               q.add(getId(r, c - 1, col));
                           }
                           if(c + 1 < col && grid[r][c + 1] == '1') {
                               grid[r + 1][j] = '2';
                               q.add(getId(r, c + 1, col));
                           }
                       }
                   }
               }
           }
           return res;
       }
       
       private Integer getId(int i, int j, int col) {
           return i * col + j;
       }
   }
   ```

   

3. 并查集，感觉不太适合解这道题，但题解**将2维数组映射为1维数组**值得学习。时间和空间O(M * N)

   ```java
   class Solution {
     class UnionFind {
       int count; // # of connected components
       int[] parent;
       int[] rank;
   
       public UnionFind(char[][] grid) { // for problem 200
         count = 0;
         int m = grid.length;
         int n = grid[0].length;
         parent = new int[m * n];
         rank = new int[m * n];
         for (int i = 0; i < m; ++i) {
           for (int j = 0; j < n; ++j) {
             if (grid[i][j] == '1') {
               parent[i * n + j] = i * n + j;
               ++count;
             }
             rank[i * n + j] = 0;
           }
         }
       }
   
       public int find(int i) { // path compression
         if (parent[i] != i) parent[i] = find(parent[i]);
         return parent[i];
       }
   
       public void union(int x, int y) { // union with rank
         int rootx = find(x);
         int rooty = find(y);
         if (rootx != rooty) {
           if (rank[rootx] > rank[rooty]) {
             parent[rooty] = rootx;
           } else if (rank[rootx] < rank[rooty]) {
             parent[rootx] = rooty;
           } else {
             parent[rooty] = rootx; rank[rootx] += 1;
           }
           --count;
         }
       }
   
       public int getCount() {
         return count;
       }
     }
   
     public int numIslands(char[][] grid) {
       if (grid == null || grid.length == 0) {
         return 0;
       }
   
       int nr = grid.length;
       int nc = grid[0].length;
       int num_islands = 0;
       UnionFind uf = new UnionFind(grid);
       for (int r = 0; r < nr; ++r) {
         for (int c = 0; c < nc; ++c) {
           if (grid[r][c] == '1') {
             grid[r][c] = '0';
             if (r - 1 >= 0 && grid[r-1][c] == '1') {
               uf.union(r * nc + c, (r-1) * nc + c);
             }
             if (r + 1 < nr && grid[r+1][c] == '1') {
               uf.union(r * nc + c, (r+1) * nc + c);
             }
             if (c - 1 >= 0 && grid[r][c-1] == '1') {
               uf.union(r * nc + c, r * nc + c - 1);
             }
             if (c + 1 < nc && grid[r][c+1] == '1') {
               uf.union(r * nc + c, r * nc + c + 1);
             }
           }
         }
       }
   
       return uf.getCount();
     }
   }
   ```

   