#  110. Balanced Binary Tree 

1. 返回左右子树的高度，若子树不平衡则返回-1，空间O(H)，时间O(N)

   ```java
   public boolean isBalanced(TreeNode root) {
       int[] temp = help(root);
       return temp[0] != -1 && temp[1] != -1 && Math.abs(temp[0] - temp[1]) <= 1;
   }
   
   private int[] help(TreeNode node) {
       if(node == null) {
           return new int[]{0, 0};
       }
       int[] res = new int[2];
       int[] left = help(node.left);
       setSubTreeHeight(left, res, 0);
       int[] right = help(node.right);
       setSubTreeHeight(right, res, 1);
       return res;
   }
   
   private void setSubTreeHeight(int[] arr, int[] res, int index) {
       if(arr[0] == -1 || arr[1] == -1 || Math.abs(arr[0] - arr[1]) > 1) {
           res[index] = -1;
       } else {
           res[index] = Math.max(arr[0], arr[1]) + 1;
       }
   }
   ```

   

2. 其实不需要知道左右子树的高度，只需要知道当前树的高度，不平衡则返回-1，空间O(H)，时间O(N)

   ```java
   public boolean isBalanced(TreeNode root) {
       return help(root) != -1;
   }
   
   public int help(TreeNode root){
       if(root == null){
           return 0;
       }
       int left = help(root.left);
       int right = help(root.right);
       if(left == -1 || right == -1 || Math.abs(left - right) > 1){
           return -1;
       }
       return 1 + Math.max(left, right);
   }
   ```

   