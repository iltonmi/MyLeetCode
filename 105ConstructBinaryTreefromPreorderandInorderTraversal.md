#  105. Construct Binary Tree from Preorder and Inorder Traversal

1. 直接递归, 空间O(H), 时间O(N^2)

   时间主要花费在从inorder中查找根节点。

   ```java
   public TreeNode buildTree(int[] preorder, int[] inorder) {
       if(preorder == null || preorder.length == 0) {
           return null;
       }
       TreeNode res = new TreeNode(preorder[0]);
       help(preorder, inorder, res, 0, preorder.length - 1, 0, inorder.length - 1);
       return res;
   }
   
   private void help(int[] preorder, int[] inorder, TreeNode head, int preLeft
                     , int preRight, int inLeft, int inRight) {
       int i = inLeft;
       for(; i <= inRight; i++) {
           if(inorder[i] == head.val) {
               break;
           }
       }
       if(inLeft < i) {
           //有左子树
           head.left = new TreeNode(preorder[preLeft + 1]);
           help(preorder, inorder, head.left, preLeft + 1, preLeft + (i - inLeft), inLeft, i - 1);
           if(i < inRight) {
               //有右子树
               head.right = new TreeNode(preorder[preLeft + i - inLeft + 1]);
               help(preorder, inorder, head.right, preLeft + (i - inLeft) + 1, preRight, i + 1, inRight);
           }
       } else {
           //没有左子树
           if(i < inRight) {
               //有右子树
               head.right = new TreeNode(preorder[preLeft + 1]);
               help(preorder, inorder, head.right, preLeft + 1, preRight, inLeft + 1, inRight);
           }
       }
   }
   ```

   

2. 使用散列表, 空间O(N), 时间O(N)

   因为我们已知数组中的元素没有重复，因此可以使用散列表储存inorder中的val -> index，节省从inorder数组查找根节点的时间。

   ```java
   class Solution {
       Map<Integer, Integer> map = new HashMap<Integer, Integer>();
       public TreeNode buildTree(int[] preorder, int[] inorder) {
           if(preorder == null || preorder.length == 0) {
               return null;
           }
           for(int i = 0; i < inorder.length; i++) {
               map.put(inorder[i], i);
           }
           TreeNode res = new TreeNode(preorder[0]);
           help(preorder, inorder, res, 0, preorder.length - 1, 0, inorder.length - 1);
           return res;
       }
       
       private void help(int[] preorder, int[] inorder, TreeNode head, int preLeft, int preRight
                        , int inLeft, int inRight) {
           int i = map.get(head.val);
           if(inLeft < i) {
               //有左子树
               head.left = new TreeNode(preorder[preLeft + 1]);
               help(preorder, inorder, head.left, preLeft + 1, preLeft + (i - inLeft), inLeft, i - 1);
               if(i < inRight) {
                   //有右子树
                   head.right = new TreeNode(preorder[preLeft + i - inLeft + 1]);
                   help(preorder, inorder, head.right, preLeft + (i - inLeft) + 1, preRight, i + 1, inRight);
               }
           } else {
               //没有左子树
               if(i < inRight) {
                   //有右子树
                   head.right = new TreeNode(preorder[preLeft + 1]);
                   help(preorder, inorder, head.right, preLeft + 1, preRight, inLeft + 1, inRight);
               }
           }
       }
   }
   ```

   