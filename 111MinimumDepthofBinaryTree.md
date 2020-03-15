#  111. Minimum Depth of Binary Tree 

注意：

1. 叶子节点：必须是左右节点都为null的节点。

2. 深度：叶子节点到根节点的路径长度。

如果有一个节点，左孩子为null而右孩子不为null，那么它的最小深度就是右子树的最小深度+1。

原因是它的左孩子不是一个叶子节点，所以从null到根节点的路径长度不符合深度的定义。

```java
public int minDepth(TreeNode root) {
    if(root == null) {
        return 0;
    }
    //如果左右都为null，那么进入这个分支不影响正确性
    if(root.left == null) {
        return minDepth(root.right) + 1;
    }
    if(root.right == null) {
        return minDepth(root.left) + 1;
    }
    return Math.min(minDepth(root.left), minDepth(root.right)) + 1;   
}
```

