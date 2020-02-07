#  101. Symmetric Tree

https://leetcode.com/articles/symmetric-tree/ 

1. 递归, 空间O(H), 时间O(N)

   ```java
   public boolean isSymmetric(TreeNode root) {
       return root == null || help(root.left, root.right);
   }
   
   private boolean help(TreeNode left, TreeNode right) {
       if((left == null ^ right == null)) {
           return false;
       }
       if(left == null) {
           return true;
       } else if(left.val != right.val) {
           return false;
       }
       return help(left.left, right.right) && help(left.right, right.left);
   }
   ```

   > ```java
   > public boolean isSymmetric(TreeNode root) {
   >     return isMirror(root, root);
   > }
   > 
   > public boolean isMirror(TreeNode t1, TreeNode t2) {
   >     if (t1 == null && t2 == null) return true;
   >     if (t1 == null || t2 == null) return false;
   >     return (t1.val == t2.val)
   >         && isMirror(t1.right, t2.left)
   >         && isMirror(t1.left, t2.right);
   > }
   > ```

2. 迭代, 空间O(H), 时间O(N)

   ```java
   public boolean isSymmetric(TreeNode root) {
       if(root == null) {
           return true;
       }
       if(root.left == null ^ root.right == null) {
           return false;
       }
       if(root.left == null) {
           return true;
       } else if(root.left.val != root.right.val) {
           return false;
       }
       Stack<TreeNode[]> stk = new Stack<>();
       stk.push(new TreeNode[]{root.left, root.right});
       while(!stk.isEmpty()) {
           TreeNode[] nodes = stk.pop();
           TreeNode left = nodes[0];
           TreeNode right = nodes[1];
           if((left == null ^ right == null)) {
               return false;
           }
           if(left == null) {
               continue;
           } else if(left.val != right.val) {
               return false;
           }
           stk.push(new TreeNode[]{left.left, right.right});
           stk.push(new TreeNode[]{left.right, right.left});
       }
       return true;
   }
   ```

   > ```java
   > public boolean isSymmetric(TreeNode root) {
   >     Queue<TreeNode> q = new LinkedList<>();
   >     q.add(root);
   >     q.add(root);
   >     while (!q.isEmpty()) {
   >         TreeNode t1 = q.poll();
   >         TreeNode t2 = q.poll();
   >         if (t1 == null && t2 == null) continue;
   >         if (t1 == null || t2 == null) return false;
   >         if (t1.val != t2.val) return false;
   >         q.add(t1.left);
   >         q.add(t2.right);
   >         q.add(t1.right);
   >         q.add(t2.left);
   >     }
   >     return true;
   > }
   > ```

3. 层次遍历，双端队列储存, 空间O(H), 时间O(N)。