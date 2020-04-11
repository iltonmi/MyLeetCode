#  257. Binary Tree Paths

1. 递归，回溯，就是处理字符串要注意一点。

2. 迭代，用String做可以，但不能用StringBuilder做（有后效性，返回之前需要delete本次函数调用添加的字符）

空间O(H)，时间O(N)。

```java
class Solution {
    private StringBuilder sb = new StringBuilder();
    private List<String> res = new LinkedList<>();
    public List<String> binaryTreePaths(TreeNode root) {
        help(root);
        return res;
    }

    private void help(TreeNode node) {
        if(node == null) {
            return;
        }
        String val = String.valueOf(node.val);
        sb.append(val);
        if(node.left == null && node.right == null) {
            res.add(sb.toString());
            removeTail(val.length());
            return;
        }
        sb.append("->");
        help(node.left);
        help(node.right);
        removeTail(val.length() + 2);
    }
    
    private void removeTail(int len) {
        int size = sb.length();
        sb.delete(size - len, size);
    }
}
```

