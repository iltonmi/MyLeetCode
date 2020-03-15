#  112. Path Sum 

注意：

1. 叶子节点：必须是左右节点都为null的节点。
2. 路径：叶子节点到根节点的路径。

如果有一个节点，左孩子为null而右孩子不为null，即使到它刚好是path sum也不符合题意。

原因是它不是一个叶子节点，所以从它到根节点的路径长度不符合路径的定义。

```java
public boolean hasPathSum(TreeNode root, int sum) {
    if(root == null) {
        return false;
    } 
    int res = sum - root.val;
    if(root.left == null && root.right == null) {
        return res == 0;
    }
    return hasPathSum(root.left, res) || hasPathSum(root.right, res);
}
```

