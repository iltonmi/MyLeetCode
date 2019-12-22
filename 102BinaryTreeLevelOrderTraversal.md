#  102. Binary Tree Level Order Traversal

1.  使用队列遍历, 空间O(N), 时间O(N)

   ```java
   class Solution {
       Queue<TreeNode> queue = new LinkedList<>();
       List<List<Integer>> res = new LinkedList<>();
       public List<List<Integer>> levelOrder(TreeNode root) {
           if(root == null) {
               return res;
           }
           queue.add(root);
           TreeNode flag = new TreeNode(-1);
           queue.add(flag);
           while(queue.peek() != flag) {
               List<Integer> curLevel = new LinkedList<Integer>();
               TreeNode lastLevelNode = null;
               while((lastLevelNode = queue.poll()) != flag) {
                   if(lastLevelNode.left != null) {
                       queue.add(lastLevelNode.left);
                   }
                   if(lastLevelNode.right != null) {
                       queue.add(lastLevelNode.right);
                   }
                   curLevel.add(lastLevelNode.val);
               }
               queue.add(flag);
               res.add(curLevel);
           }
           return res;
       }
   }
   ```

   

2. 递归, 空间O(N), 时间O(N)

   ```java
   class Solution {
       List<List<Integer>> res = new ArrayList<>();
       public List<List<Integer>> levelOrder(TreeNode root) {
           if(root == null) {
               return res;
           }
           help(root, 0);
           return res;
       }
       
       private void help(TreeNode node, int level) {
           if(res.size() == level) {
               res.add(new LinkedList<>());
           }
           res.get(level).add(node.val);
           if(node.left != null) {
               help(node.left, level + 1);
           }
           if(node.right != null) {
               help(node.right, level + 1);
           }
       }
   }
   ```

   