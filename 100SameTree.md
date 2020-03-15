#  100. Same Tree 

1. 递归，空间O(H)，时间O(N)

   ```java
   public boolean isSameTree(TreeNode p, TreeNode q) {
       return help(p, q);
   }
   
   private boolean help(TreeNode p, TreeNode q) {
       if(p == null && q == null) {
           return true;
       }
       //到这里, p和q不可能同时为null
       if(p == null || q == null || p.val != q.val) {
           return false;
       }
   
       return help(p.left, q.left) && help(p.right, q.right);
   }
   ```

   

2. 迭代，空间O(H)，时间O(N)

   ```java
   public boolean isSameTree(TreeNode p, TreeNode q) {
       Deque<TreeNode[]> stk = new LinkedList<>();
       stk.push(new TreeNode[]{p, q});
       while(!stk.isEmpty()) {
           TreeNode[] param = stk.pop();
           TreeNode node1 = param[0];
           TreeNode node2 = param[1];
           if(node1 == null && node2 == null) {
               continue;
           }
           //到这里, p和q不可能同时为null
           if(node1 == null || node2 == null || node1.val != node2.val) {
               return false;
           }
           stk.push(new TreeNode[]{node1.right, node2.right});
           stk.push(new TreeNode[]{node1.left, node2.left});
       }
       return true;
   }
   ```

   