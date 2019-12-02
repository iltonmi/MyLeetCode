# 437. Path Sum III

1. 递归，空间O(h), 时间
   $$
   O(n^2)
   $$
   

   ```java
   /**
    * Definition for a binary tree node.
    * public class TreeNode {
    *     int val;
    *     TreeNode left;
    *     TreeNode right;
    *     TreeNode(int x) { val = x; }
    * }
    */
   public int pathSum(TreeNode root, int sum) {
       if(root == null) {
           return 0;
       }
       return rootStartPathSum(root, sum) +
           pathSum(root.left, sum) +
           pathSum(root.right, sum);
   }
   
   private int rootStartPathSum(TreeNode root, int sum) {
       if(root == null) {
           return 0;
       }
       int nextSum = sum - root.val;
       return (nextSum == 0 ? 1 : 0) +
           rootStartPathSum(root.left, nextSum) +
           rootStartPathSum(root.right, nextSum);
   }
   ```

   

2. DFS迭代

   ```java
   public int pathSum(TreeNode root, int sum) {
       if(root == null) {
           return 0;
       }
       int res = 0;
       Stack<TreeNode> nodes = new Stack<>();
       nodes.push(root);
       while(!nodes.isEmpty()) {
           TreeNode node = nodes.pop();
           if(node == null) {
               continue;
           }
           res += rootStartPathSum(node, sum);
           nodes.push(node.right);
           nodes.push(node.left);
       }
       return res;
   }
   
   private int rootStartPathSum(TreeNode root, int sum) {
       if(root == null) {
           return 0;
       }
       int res = 0;
       Stack<Object[]> params = new Stack<>();
       params.push(new Object[]{root, Integer.valueOf(sum)});
       while(!params.isEmpty()) {
           Object[] param = params.pop();
           if(param[0] == null) {
               continue;
           }
           TreeNode curNode = (TreeNode) param[0];
           Integer curSum = (Integer) param[1];
           int nextSum = curSum - curNode.val;
           if(nextSum == 0) {
               res++;
           }
           params.push(new Object[]{curNode.right, Integer.valueOf(nextSum)});
           params.push(new Object[]{curNode.left, Integer.valueOf(nextSum)});
       }
       return res;
   }
   ```

3. 使用hash表，时间O(n), 空间O(h)

   1. 问题联想：

      求数组中，有多少子数组的总值是target？

   2. 假设 sum[i] = a[0...i], 则sum[k] = a[0...k]。若 sum[k]-sum[i] = target, 则i..k为一个解。

   3. 如何快速找到，有多少个 sum[i] 符合 sum[k]-sum[i] = target 呢？

   4. 准备工作：

      在遍历过程中，对于a[i]，使用 hash table：若key=sum[i] 不存在，存储 <sum[i], 1>；否则，将以sum[i]为key的value自增1。这个value的意义就是有总值为sum[i]的子数组的数量。

   5. 具体例子：

   遍历数组到index i。利用hash table, 以 key 为 sum[k] - target 快速查找前面总值为 sum[k] - target 的子数组数量，这个数量=以a[i]结尾且总值为target的数组数量。

   6. 回到本问题，一条path上有多少子路径的总值为target和一个数组中有多少子数组的总值为target非常类似。

   ```java
   public int pathSum(TreeNode root,int sum){
       HashMap map = new HashMap<>();
       map.put(0, 1);
       return pathSumUsingHashMap(root, sum, map, 0);
   }
   
   private int pathSumUsingHashMap(
       TreeNode root, int target, HashMap<Integer, Integer> map, int preSum) {
   
       if(root == null) {
           return 0;
       }
   
       int curSum = preSum + root.val;
       int res = map.getOrDefault(curSum - target, 0);
       map.put(curSum, map.getOrDefault(curSum, 0) + 1);
       res += pathSumUsingHashMap(root.left, target, map, curSum);
       res += pathSumUsingHashMap(root.right, target, map, curSum);
       map.put(curSum, map.get(curSum) - 1);
       return res;
   }
   ```

   