#  103. Binary Tree Zigzag Level Order Traversal 

1. 双端队列，下一层的末尾就是上一层第一个被加入队列的孩子节点，空间O(N/2) = O(N)，时间O(N)

   ```java
   public List<List<Integer>> zigzagLevelOrder(TreeNode root) {
       List<List<Integer>> res = new ArrayList<>();
       if(root == null) {
           return res;
       }
       boolean ltor = true;
       Deque<TreeNode> dq = new ArrayDeque<>();
       dq.offer(root);
       TreeNode last = root;
       TreeNode nLast = null;
       List<Integer> curLevel = new ArrayList<>();
       while(!dq.isEmpty()) {
           TreeNode node;
           if(ltor) {
               node = dq.pollFirst();
               if(node.left != null) {
                   nLast = nLast != null ? nLast : node.left;
                   dq.offerLast(node.left);
               }
               if(node.right != null) {
                   nLast = nLast != null ? nLast : node.right;
                   dq.offerLast(node.right);
               }
           } else {
               node = dq.pollLast();
               if(node.right != null) {
                   nLast = nLast != null ? nLast : node.right;
                   dq.offerFirst(node.right);
               }
               if(node.left != null) {
                   nLast = nLast != null ? nLast : node.left;
                   dq.offerFirst(node.left);
               }
           }
           curLevel.add(node.val);
           if(node == last) {
               last = nLast;
               nLast = null;
               ltor = !ltor;
               res.add(curLevel);
               curLevel = new ArrayList<>();
           }
       }
       return res;
   }
   ```

   