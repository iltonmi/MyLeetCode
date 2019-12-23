#  114. Flatten Binary Tree to Linked List 

1. 原地迭代, 空间O(1), 时间O(N)

   每次处理一棵树的问题，将右子树拼接到左子树的右边界节点形成一棵新树，然后处理左子树的问题。（出于题目要求，拼接后需要将左子树根节点赋值给根节点的右子树）

   **为何递归改写为迭代不需要用栈？**实际上，例如中序遍历，需要用栈是因为**左子树处理完毕之后，无法找到指向根节点的引用，才需要用栈保存根节点**，但在**此题中，无需再次访问根节点**，因此不需要用栈。

   ```java
   public void flatten(TreeNode root) { 
       TreeNode rm = null;
       while(root != null) {
           rm = root.left;
           if(rm != null) {
               while(rm.right != null) {
                   rm = rm.right;
               }
               rm.right = root.right;
               root.right = root.left;
               root.left = null;
           }
           root = root.right;
       }
   }
   ```

   

2. 递归, 空间O(N), 时间O(N)

   ```java
   public void flatten(TreeNode root) { 
       if (root == null) return;
       TreeNode rm = root.left;
       if(rm != null) {
           while(rm.right != null) {
               rm = rm.right;
           }
           rm.right = root.right;
           root.right = root.left;
           root.left = null;
       }
       flatten(root.right);
   }
   ```

   