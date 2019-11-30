 # 104. Maximum Depth of Binary Tree 



1. 递归，O(m)，m为最大深度

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
       public int maxDepth(TreeNode root) {
           if(root == null) {
               return 0;
           }
           return help(root, 0);
       }
       
       private int help(TreeNode root, int depth) {
           if(root == null) {
               return depth;
           }
           depth++;
           return Math.max(help(root.left, depth), help(root.right, depth));
       }
   }
   ```

   

2. DFS

```java
    public int maxDepth(TreeNode root) {
        if(root == null) {
            return 0;
        }
        int res = 0;
        Stack<TreeNode> nodes = new Stack<>();
        Stack<Integer> depths = new Stack<>();
        nodes.push(root);
        depths.push(0);
        while(!nodes.isEmpty()) {
            TreeNode cur = nodes.pop();
            Integer prevDepth = depths.pop();
            if(cur == null) {
                continue;
            }
            Integer curDepth = prevDepth + 1;
            res = Math.max(res, curDepth);
            nodes.push(cur.right);
            depths.push(curDepth);
            nodes.push(cur.left);
            depths.push(curDepth);
        }
        return res;
    }
```

3. BFS

   ```java
       public int maxDepth(TreeNode root) {
           if(root == null) {
               return 0;
           }
           int res = 0;
           Queue<TreeNode> nodes = new LinkedList<>();
           Queue<Integer> depths = new LinkedList<>();
           nodes.add(root);
           depths.add(0);
           while(!nodes.isEmpty()) {
               TreeNode cur = nodes.remove();
               Integer prevDepth = depths.remove();
               if(cur == null) {
                   continue;
               }
               Integer curDepth = prevDepth + 1;
               res = Math.max(res, curDepth);
               nodes.add(cur.left);
               depths.add(curDepth);
               nodes.add(cur.right);
               depths.add(curDepth);
           }
           return res;
       }
   ```

   

   