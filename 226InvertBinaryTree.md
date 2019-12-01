#  226. Invert Binary Tree 

1. 递归，时间O(n), 空间O(h)

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
   public TreeNode invertTree(TreeNode root) {
       if(root == null) {
           return null;
       }
       TreeNode tempLeft = root.left;
       root.left = invertTree(root.right);
       root.right = invertTree(tempLeft);
       return root;
   }
   ```

   

2. DFS迭代，时间O(n), 空间O(h)

   ```java
    public TreeNode invertTree(TreeNode root) {
       Stack<TreeNode> stk = new Stack<>();
       stk.push(root);
       while(!stk.isEmpty()) {
           TreeNode cur = stk.pop();
           if(cur == null) {
               continue;
           }
           TreeNode tempLeft = cur.left;
           cur.left = cur.right;
           cur.right = tempLeft;
           stk.push(cur.right);
           stk.push(cur.left);
       }
       return root;
   }
   ```

3. BFS

   ```java
   public TreeNode invertTree(TreeNode root) {
       if (root == null) return null;
       Queue<TreeNode> queue = new LinkedList<TreeNode>();
       queue.add(root);
       while (!queue.isEmpty()) {
           TreeNode current = queue.poll();
           TreeNode temp = current.left;
           current.left = current.right;
           current.right = temp;
           if (current.left != null) queue.add(current.left);
           if (current.right != null) queue.add(current.right);
       }
       return root;
   }
   ```

   

