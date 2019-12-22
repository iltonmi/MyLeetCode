#  96. Unique Binary Search Trees

 https://leetcode-cn.com/problems/unique-binary-search-trees/solution/bu-tong-de-er-cha-sou-suo-shu-by-leetcode/ 

1. 动态规划, 

   1. 定义2个函数：

   $$
   G(n): 长度为n的序列的不同二叉搜索树个数。
   $$

   $$
   F(i,n): 以i为根的不同二叉搜索树个数(1 \leq i \leq n1≤i≤n)。
   $$

   2. 函数关系式：
      $$
      G(n) = \sum_{i=1}^{n}F(i,n), 且F(i,n)=G(i−1)⋅G(n−i)
      $$

   3. 结论

   $$
   G(n) = \sum_{i=1}^{n}G(i-1)G(n-i),其中:G(0)=G(1)=1
   $$

   $$
   即，G(n) = G(0) * G(n-1) + G(1) * G(n-2) + … + G(n-1) * G(0)
   $$

   4. 复杂度分析：

      时间复杂度：
      $$
      \sum_{i=2}^ni=\frac{(n+2)(n-1)}{2}=O(N^2)
      $$ {、}
      空间复杂度：
      $$
      O(N)
      $$
      

   代码如下：

   ```java
   public int numTrees(int n) {
       if(n <= 1) {
           return 1;
       }
       int[] g = new int[n + 1];
       g[0] = 1;
       g[1] = 1;
       for(int i = 2; i <= n; i++) {
           for(int j = 1; j <= i; j++) {
               g[i] += g[j-1] * g[i-j];
           }
       }
       return g[n];
   }
   ```

   

2. 卡特兰数, 空间O(1), 时间O(N)

    令h(0)=1,h(1)=1，卡塔兰数满足递归式：
   $$
   h(n)= h(0)*h(n-1) + h(1)*h(n-2) + ... + h(n-1)*h(0) (其中n>=2),这是n阶递推关系
   $$
    **还可以化简为1阶递推关系:** 
   $$
   h(n)=\frac{2(2n+1)}{(n+2)}*h(n-1) (n>=1), h(0)=1
   $$

    ```java
   public int numTrees(int n) {
       long C = 1;
       for (int i = 1; i < n; ++i) {
           C = C * 2 * (2 * i + 1) / (i + 2);
       }
       return (int) C;
   }
    ```

   