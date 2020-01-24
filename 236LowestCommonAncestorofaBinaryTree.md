#  236. (mark)Lowest Common Ancestor of a Binary Tree

主要解决的问题是，如何记录父节点。

1. 后序遍历，时间O(N)，空间O(H)

   利用后序遍历的返回值，既能表示哪个分支查找成功，也能返回查找到的节点。

   1. 若节点为空或者是p或q，直接返回。
   2. 若2棵后续遍历的返回值都不为空，则当前节点则是最近公共祖先，直接返回当前节点。
   3. 若其中一个为空，则可能是答案或者其中一个节点，直接返回即可。

   ```java
   class Solution {
       
       public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
           if(root == null || root == p || root == q) {
               return root;
           }
           TreeNode left = lowestCommonAncestor(root.left, p, q);
           TreeNode right = lowestCommonAncestor(root.right, p, q);
           if(left != null && right != null) {
               return root;
           }
           return left != null ? left : right;
       }
   }
   ```

   

2. 使用散列表和集合，时间O(N)，空间O(N)

   遍历树过程中，使用散列表保存 node - > parent

   利用散列表，找到p的全部祖先，并储存到集合中。

   利用散列表，遍历q的祖先，查询其是否在集合中。

   ```java
   class Solution {
       
       public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
           TreeNode head = root;
           Deque<TreeNode> stk = new ArrayDeque<>();
           Map<TreeNode, TreeNode> map = new HashMap<>();
           stk.push(head);
           while(!stk.isEmpty()) {
               TreeNode node = stk.pop();
               if(node.left != null) {
                   stk.push(node.left);
                   map.put(node.left, node);
               }
               if(node.right != null) {
                   stk.push(node.right);
                   map.put(node.right, node);
               }
           }
           Set<TreeNode> set = new HashSet<>();
           while(p != null) {
               set.add(p);
               p = map.get(p);
           }
           while(q != null) {
               if(set.contains(q)) {
                   return q;
               }
               q = map.get(q);
           }
           return null;
       }
   }
   ```

   

扩展问题：快速查询，批量查询。