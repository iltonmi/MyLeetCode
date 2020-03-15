#  107. Binary Tree Level Order Traversal II 

1. 常规操作，空间O(N/2)，时间O(N)

   ```java
   public List<List<Integer>> levelOrderBottom(TreeNode root) {
       Deque<List<Integer>> res = new LinkedList<>();
       if(root == null) {
           return (List<List<Integer>>) res;
       }
       List<TreeNode> preLevel = new LinkedList<>();
       preLevel.add(root);
       List<Integer> curRes = new LinkedList<>();
       curRes.add(root.val);
       res.push(curRes);
   
       while(true) {
           List<TreeNode> curLevel = new LinkedList<>();
           curRes = new LinkedList<>();
           for(TreeNode node : preLevel) {
               if(node.left != null) {
                   curLevel.add(node.left);
                   curRes.add(node.left.val);
               }
               if(node.right != null) {
                   curLevel.add(node.right);
                   curRes.add(node.right.val);
               }
           }
           if(curRes.isEmpty()) {
               break;
           }
           res.push(curRes);
           preLevel = curLevel;
       }
       return (List<List<Integer>>) res;
   }
   ```

   