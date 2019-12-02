#  543. Diameter of Binary Tree

1. DFS，时间O(n)，空间O(h)

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
   class Solution {
       int res;
       public int diameterOfBinaryTree(TreeNode root) {
           res = 0;
           depth(root);
           return res;
       }
       /**
       * 返回节点深度
       **/
       public int depth(TreeNode node) {
           //边界条件
           if(node == null || (node.left == null && node.right == null)) {
               return 0;
           }
           //到这一步，此节点必然不为null且至少有一棵子树
           int L = depth(node.left);
           int R = depth(node.right);
           //最大子树深度
           int childDepth = Math.max(L, R);
           //经过此节点的最长路径=左子树深度+右子树深度+(连接子树的路径数，只有一棵子树则为1，有两棵则为2)
           res = Math.max(res, L + R + ((node.left == null || node.right == null) ? 1 : 2));
           //此节点深度为最大子树深度+1
           return childDepth + 1;
       }
   }
   ```

   