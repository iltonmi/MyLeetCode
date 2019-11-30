# 617. Merge Two Binary Trees
**大致分为是否创建新树两种，取决于具体情况**
## 不创建新树两种解法

1. 使用递归进行前序遍历，时间O(m), 空间复杂度O(m), m为两颗树的节点数中最小的一个。
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
public class Solution {
    public TreeNode mergeTrees(TreeNode t1, TreeNode t2) {
        if (t1 == null)
            return t2;
        if (t2 == null)
            return t1;
        t1.val += t2.val;
        t1.left = mergeTrees(t1.left, t2.left);
        t1.right = mergeTrees(t1.right, t2.right);
        return t1;
    }
}
```
2.使用栈通过迭代方式前序遍历，时间、空间和递归一样
```java
class Solution {
    public TreeNode mergeTrees(TreeNode t1, TreeNode t2) {
        if(t1 == null) {
            return t2;
        }
        
        Stack<TreeNode[]> stk = new Stack<>();
        stk.push(new TreeNode[] {t1, t2});
        while(!stk.isEmpty()) {
            TreeNode[] pair = stk.pop();
            if(pair[1] == null) {
                continue;
            }
            
            TreeNode n1 = pair[0];
            TreeNode n2 = pair[1];
            n1.val += n2.val;
            if(n1.right == null) {
                n1.right = n2.right;
            } else {
                stk.push(new TreeNode[] {n1.right, n2.right});
            }
            if(n1.left == null) {
                n1.left = n2.left;
            } else {
                stk.push(new TreeNode[] {n1.left, n2.left});
            }
        }
        return t1;
    }
}
```

## 创建新树解法

1. 递归
```java
class Solution {
    public TreeNode mergeTrees(TreeNode t1, TreeNode t2) {
        if(t1 == null && t2 == null) {
            return null;
        }
        
        int val = (t1 == null ? 0 : t1.val) + (t2 == null ? 0 : t2.val);
        TreeNode res = new TreeNode(val);
        res.left = mergeTrees(t1 == null ? null : t1.left, t2 == null ? null : t2.left);
        res.right = mergeTrees(t1 == null ? null : t1.right, t2 == null ? null : t2.right);
        
        return res;
    }
}
```
2. 迭代
```java
class Solution {
    public TreeNode mergeTrees(TreeNode t1, TreeNode t2) {
        if (t1 == null && t2 == null) {
            return null;
        }
        Stack<TreeNode[]> stk = new Stack<>();
        TreeNode res = new TreeNode((t1 == null ? 0 : t1.val) + (t2 == null ? 0 : t2.val));
        stk.push(new TreeNode[] {t1, t2, res});
        while(!stk.isEmpty()) {
            TreeNode[] pair = stk.pop();
            if(pair[0] == null && pair[1] == null) {
                continue;
            }
            
            TreeNode n1 = pair[0];
            TreeNode n2 = pair[1];
            TreeNode p = pair[2];
            if(n1 != null && n2 != null) {
                if (n1.right != null || n2.right != null) {
                    int rightVal = (n1.right == null ? 0 : n1.right.val) +
                    (n2.right == null ? 0 : n2.right.val);
                    p.right = new TreeNode(rightVal);
                    stk.push(new TreeNode[]{n1.right, n2.right, p.right});
                }
                if (n1.left != null || n2.left != null) {
                    int leftVal = (n1.left == null ? 0 : n1.left.val) +
                    (n2.left == null ? 0 : n2.left.val);
                    p.left = new TreeNode(leftVal);
                    stk.push(new TreeNode[]{n1.left, n2.left, p.left});
                }
            } else if (n1 != null) {
                if(n1.right != null) {
                    p.right = new TreeNode(n1.right.val);
                    stk.push(new TreeNode[]{n1.right, null, p.right});
                }
                if(n1.left != null) {
                    p.left = new TreeNode(n1.left.val);
                    stk.push(new TreeNode[]{n1.left, null, p.left});
                }
            } else {
                if(n2.right != null) {
                    p.right = new TreeNode(n2.right.val);
                    stk.push(new TreeNode[]{null, n2.right, p.right});
                }
                if(n2.left != null) {
                    p.left = new TreeNode(n2.left.val);
                    stk.push(new TreeNode[]{null, n2.left, p.left});
                }
            }
        
        }
        return res;
    }
}
```