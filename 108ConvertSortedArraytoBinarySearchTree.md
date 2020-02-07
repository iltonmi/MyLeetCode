# 108. Convert Sorted Array to Binary Search Tree

1. 二分查找根节点，递归，空间O(logN)，空间O(N)

   ```java
   public TreeNode sortedArrayToBST(int[] num) {
       return helper(num, 0, num.length - 1);
   }
   
   public TreeNode helper(int[] num, int lo, int hi) {
       if (lo > hi) { // Done
           return null;
       }
       int mid = (hi - lo) / 2 + lo;
       TreeNode node = new TreeNode(num[mid]);
       node.left = helper(num, lo, mid - 1);
       node.right = helper(num, mid + 1, hi);
       return node;
   }
   ```

   

2. BFS, DFS，时间空间不变。从容器将节点取出来后，用二分查找左右节点值的索引，创建并连接左右节点，然后将左右节点加进容器。

   