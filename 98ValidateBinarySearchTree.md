#  98. Validate Binary Search Tree 

1. 递归, 空间O(h), 时间O(N)

   ```java
   class Solution {
       public boolean helper(TreeNode node, Integer lower, Integer upper) {
           if(node == null) {
               return true;
           }   
           int val = node.val;
           if(lower != null && lower >= val) {
               return false;
           }
           if(upper != null && val >= upper) {
               return false;
           }
           if(!helper(node.left, lower, val)) {
               return false;
           }
           if(!helper(node.right, val, upper)) {
               return false;
           }
           return true;
       }
   
       public boolean isValidBST(TreeNode root) {
           return helper(root, null, null);
       }
   }
   ```

   

2. 迭代, 空间O(h), 时间O(N)

   ```java
   class Solution {
       Deque<TreeNode> stk = new LinkedList<>();
       Deque<Integer> uppers = new LinkedList<>(),
             lowers = new LinkedList();
       
       public boolean isValidBST(TreeNode root) {
           stk.push(root);
           uppers.push(null);
           lowers.push(null);
           while(!stk.isEmpty()) {
               TreeNode node = stk.pop();
               Integer upper = uppers.pop();
               Integer lower = lowers.pop();
               
               if(node == null) {
                   continue;
               }
               int val = node.val;
               if(lower != null && lower >= val) {
                   return false;
               }
               if(upper != null && val >= upper) {
                   return false;
               }
               update(node.right, val, upper);
               update(node.left, lower, val);
           }
           return true;
       }
       
       private void update(TreeNode node, Integer lower, Integer upper) {
           stk.push(node);
           lowers.push(lower);
           uppers.push(upper);
       }
   }
   ```

   

3. 利用定义, 空间O(h), 时间O(N)

   ```java
   class Solution {
       Deque<TreeNode> stk = new LinkedList<>();
       
       public boolean isValidBST(TreeNode root) {
           //rightMost：当出现root == null时，rightMost总是栈顶节点在中序遍历中的前一个节点。
           TreeNode rightMost = null;
           while(!stk.isEmpty() || root != null) {
               if(root != null) {
                   stk.push(root);
                   root = root.left;
               } else {
                   root = stk.pop();
                   //当root=root.right 导致 root==null,则 rightMost 就是栈顶节点左子树的右边界节点
                   //当root=root.left 导致 root==null,则 rightMost 是栈顶节点为最左节点的子树的父节点
                   if(rightMost != null && rightMost.val >= root.val) {
                       return false;
                   }
                   rightMost = root;
                   root = root.right;
               }
           }
           return true;
       }
   }
   ```

   