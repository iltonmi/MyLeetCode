# 230. Kth Smallest Element in a BST

中序遍历

1. 递归，空间O(N)，时间O(N)。

   ```java
   public List<Integer> inorder(TreeNode root, List<Integer> arr) {
       if (root == null) {
           return arr;
       }
       inorder(root.left, arr);
       arr.add(root.val);
       inorder(root.right, arr);
       return arr;
   }
   
   public int kthSmallest(TreeNode root, int k) {
       List<Integer> nums = inorder(root, new LinkedList<>());
       return nums.get(k - 1);
   }
   ```

   ```java
   class Solution {
       private int count;
       private int k;
       public TreeNode inorder(TreeNode root) {
           if (root == null) {
               return null;
           }
           TreeNode res = inorder(root.left);
           if(res != null) {
               return res;
           }
           if(++count == k) {
               return root;
           }
           res = inorder(root.right);
           if(res != null) {
               return res;
           }
           return null;
       }
   
       public int kthSmallest(TreeNode root, int k) {
           this.count = 0;
           this.k = k;
           TreeNode res = inorder(root);
           return res == null ? 0 : res.val;
       }
   }
   ```

   

2. 迭代，空间O(N)，时间O(N)。

   ```java
   public int kthSmallest(TreeNode root, int k) {
       Deque<TreeNode> stk = new LinkedList<>();
       int count = 0;
       while(!stk.isEmpty() || root != null) {
           if(root != null) {
               stk.push(root);
               root = root.left;
           } else {
               root = stk.pop();
               if(++count == k) {
                   return root.val;
               } 
               root = root.right;
           }
       }
       return 0;
   }
   ```

   

   ```java
   public int kthSmallest(TreeNode root, int k) {
       LinkedList<TreeNode> stack = new LinkedList<TreeNode>();
   
       while (true) {
         while (root != null) {
           stack.add(root);
           root = root.left;
         }
         root = stack.removeLast();
         if (--k == 0) return root.val;
         root = root.right;
       }
   }
   ```

   