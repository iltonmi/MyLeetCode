#  337. House Robber III 

1. 5分钟手写递归，这个时间复杂度我懵了，空间复杂度O(H)

   ```java
   class Solution {
       public int rob(TreeNode root) {
           return rob(root, false);
       }
       
       public int rob(TreeNode node, boolean parentTheft) {
           if(node == null) {
               return 0;
           }
           if(!parentTheft) {
               int stolen = node.val + rob(node.left, true) + rob(node.right, true);
               int notStolen = rob(node.left, false) + rob(node.right, false); //a1
               return Math.max(stolen, notStolen);
           } else {
               return rob(node.left, false) + rob(node.right, false); //a2
           }
           
       }
   }
   ```

   

2. 散列表记忆递归，空间O(H)，时间O(N)。

   观察上一个方法a1和a2处出现了重复子问题。

   ```java
   class Solution {
       private Map<TreeNode, Integer> map = new HashMap<>();
       public int rob(TreeNode root) {
           return rob(root, false);
       }
       
       public int rob(TreeNode node, boolean parentStolen) {
           if(node == null) {
               return 0;
           }
           //只有parentStolen为false的情况需要被储存。
           if(!parentStolen && map.containsKey(node)) {
               return map.get(node);
           }
           if(!parentStolen) {
               int stolen = node.val + rob(node.left, true) + rob(node.right, true);
               int notStolen = rob(node.left, false) + rob(node.right, false);
               return Math.max(stolen, notStolen);
           } else {
               int left = rob(node.left, false);
               int right = rob(node.right, false);
               //储存结果
               map.put(node.left, left);
               map.put(node.right, right);
               return left + right;
           }
           
       }
   }
   ```

 https://leetcode.com/problems/house-robber-iii/discuss/79330/Step-by-step-tackling-of-the-problem 

以下思路来自以上链接，前2个其实没啥明显区别，第3个是颠覆的思路。

1. 不保存父节点是否被偷的递归。，这个时间复杂度我懵了，空间复杂度O(H)。

   递归的逻辑不一样：父节点被偷后，直接取孙子节点。

   值得学习，比第1个方法优雅，参数少也容易优化。

   ```java
   class Solution {
       public int rob(TreeNode root) {
           if(root == null) {
               return 0;
           }
           int stolen = 0;
           if(root.left != null) {
               stolen += rob(root.left.left);
               stolen += rob(root.left.right);
           }
           if(root.right != null) {
              stolen += rob(root.right.left);
               stolen += rob(root.right.right); 
           }
           int notStolen = rob(root.left) + rob(root.right);
           return Math.max(root.val + stolen, notStolen);
       }
   }
   ```

   

2. 不保存父节点是否被偷的递归，记忆版，空间O(H)，时间O(N)。

   ```java
   class Solution {
       private Map<TreeNode, Integer> map = new HashMap<>();
       public int rob(TreeNode root) {
           if(root == null) {
               return 0;
           }
           if(map.containsKey(root)) {
               return map.get(root);
           }
           int stolen = 0;
           if(root.left != null) {
               stolen += rob(root.left.left);
               stolen += rob(root.left.right);
           }
           if(root.right != null) {
              stolen += rob(root.right.left);
               stolen += rob(root.right.right); 
           }
           int notStolen = rob(root.left) + rob(root.right);
           int res = Math.max(root.val + stolen, notStolen);
           map.put(root, res);
           return res;
       }
   }
   ```

   

3. 联想我们此前做递归，rob()的定义是root为根的树的最大值，我们要得到最大值，其实必然是已经比较过root被抢和不被抢2种情况的值再进行返回。

   猜想：如果我们将这2个值都返回, **返回更多的信息**，即将rob()定义为一个包含root被抢和不被抢的最大值数组，在左右子树的这2个值都返回会帮助计算root的2个值吗？

   我们来看看:

   ```java
   stolen = left[notsStolen] + right[notStolen], 
   notStolen = max(left[stolen], left[notStolen]) + max(right[stolen], right[notStolen])
   ```

   **为什么这样行得通？** 对于重复的子问题解，本来在**方法2中是由散列表储存**的，原因是在**递归回溯之后，我们无法再次访问这些重复的子问题解**，只好将其储存起来。而且，这道题有个特点，可以**被有效重复利用的解正好只处于被调用的下一层，利用递归栈**直接返回即可。

   **好处：**而且这样可以将root是否被抢的信息保存在返回值中，就不需要在参数中保存了。

   **比较：**

   我自创的**将父节点是否被抢保存在参数中**。

   而这种将子节点是否被抢的结果保存在**返回值中**的做法**隐含了子节点是否被抢**。

   ```java
   class Solution {
       public int rob(TreeNode root) {
           int[] res = robSub(root);
           return Math.max(res[0], res[1]);
       }
       
       public int[] robSub(TreeNode node) {
           int[] res = new int[2];
           if(node == null) {
               return res;
           }
           
           int[] left = robSub(node.left);
           int[] right = robSub(node.right);
           int notStolen = Math.max(left[0], left[1]) 
               + Math.max(right[0], right[1]);
           int stolen = node.val + left[0] + right[0];
           res[0] = notStolen;
           res[1] = stolen;
           return res;
       }
   }
   ```

   