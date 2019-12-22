#  94. Binary Tree Inorder Traversal

经典无需多言，重点关注莫里斯遍历。

1.  递归, 平均空间O(logN)，最大空间O(N), 时间O(N)

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
       private List<Integer> res = new LinkedList<>();
       
       public List<Integer> inorderTraversal(TreeNode root) {
           help(root);
           return res;
       }
       
       private void help(TreeNode root) {
           if(root == null) {
               return;
           }
           help(root.left);
           res.add(root.val);
           help(root.right);
       }
   }
   ```

2. 迭代, 一样

   ```java
   class Solution {
       private List<Integer> res = new LinkedList<>();
       
       public List<Integer> inorderTraversal(TreeNode root) {
           Stack<TreeNode> stk = new Stack<>();
           while(!stk.isEmpty() || root != null) {
               if(root != null) {
                   stk.add(root);
                   root = root.left;
               } else {
                   root = stk.pop();
                   res.add(root.val);
                   root = root.right;
               }
           }
           return res;
       }
   }
   ```

   

3. morris遍历, 空间O(1), 时间O(N)

   ```java
   class Solution {
       private List<Integer> res = new LinkedList<>();
    
       public List<Integer> inorderTraversal(TreeNode root) {
           TreeNode cur = root;
           TreeNode rightMost = null;
           while(cur != null) {
               rightMost = cur.left;
               if(rightMost != null) {
                   while(rightMost.right != null && rightMost.right != cur) {
                       rightMost = rightMost.right;
                   }
                   
                   if(rightMost.right == null) {
                       rightMost.right = cur;
                       cur = cur.left;
                       continue;
                   } else {
                       rightMost.right = null;
                   }
               }
               res.add(cur.val);
               cur = cur.right;
           }
           return res;
       }
   }
   ```
   
   