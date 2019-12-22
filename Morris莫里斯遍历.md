# Morris莫里斯遍历

[程序员代码面试指南（第2版）第3章 二叉树问题：遍历二叉树的神级方法]( https://item.jd.com/12518392.html )

https://leetcode.com/articles/binary-tree-inorder-traversal/

> Step 1: Initialize current as root
>
> Step 2: While current is not NULL,
>
> If current does not have left child
>
> ```java
> a. Add current’s value
> 
> b. Go to the right, i.e., current = current.right
> ```
>
> Else
>
> ```java
> If rightmost node of current's left subtree has no right child
>     a. In current's left subtree, make current the right child of the rightmost node
> 
>     b. Go to this left child, i.e., current = current.left
> Else 
> 	a. Restore the origianl state, assign null value to the right child of the rightmost node
> ```

在了解Morris序后，通过对能到达2次的节点选择性的打印，可以很直观的得到前序遍历和中序遍历。

对于Morris序，先把左子树的右边界节点和cur连上，再处理左子树。(这个查找左子树右边界节点的过程不属于Morris遍历过程。)

cur被指针保存后，就可以放心的移动了。

有左子树的节点都会到达2次，每次都需要重新查找右边界。

1. Morris遍历

   ```java
   public static void morris(Node head) {
       if (head == null) {
           return;
       }
       Node cur = head;
       //这个mostRight指的是左子树的mostRight,而不是当前节点的mostRight
       Node mostRight = null;
       while (cur != null) {
           mostRight = cur.left;
           if (mostRight != null) { // 如果当前cur有左子树
               // 找到cur左子树上最右的节点
               while (mostRight.right != null && mostRight.right != cur) {
                   mostRight = mostRight.right;
               }
               // 从上面的while里出来后，mostRight就是cur左子树上最右的节点
               if (mostRight.right == null) { // 如果mostRight.right是指向null的
                   mostRight.right = cur; // 让其指向cur
                   cur = cur.left; // cur向左移动
                   continue; // 回到最外层的while，继续判断cur的情况
               } else { // 如果mostRight.right是指向cur的
                   mostRight.right = null; // 让其指向null
               }
           }
           // cur如果没有左子树，cur向右移动
           // 或者cur左子树上最右节点的右指针是指向cur的，cur向右移动
           cur = cur.right;
       }
   }
   ```

   

2. Morris前序遍历

   对于只到达1次的节点，马上打印。

   对于到达2次的节点，只打印第1次。

   ```java
   public static void morrisPre(Node head) {
       if (head == null) {
           return;
       }
       Node cur = head;
       Node mostRight = null;
       while (cur != null) {
           mostRight = cur.left;
           if (mostRight != null) {
               //到达2次的节点
               while (mostRight.right != null && mostRight.right != cur) {
                   mostRight = mostRight.right;
               }
               if (mostRight.right == null) {
                   //2次中的第1次
                   mostRight.right = cur;
                   System.out.print(cur.value + " ");
                   cur = cur.left;
                   continue;
               } else {
                   //2次中的第2次
                   mostRight.right = null;
               }
           } else {
               //只能到达1次的节点
               System.out.print(cur.value + " ");
           }
           cur = cur.right;
       }
       System.out.println();
   }
   ```

   

3. Morris中序遍历

   对于只到达1次的节点，马上打印。

   对于到达2次的节点，只打印第2次。

   ```java
   public static void morrisIn(Node head) {
       if (head == null) {
           return;
       }
       Node cur = head;
       Node mostRight = null;
       while (cur != null) {
           mostRight = cur.left;
           if (mostRight != null) {
               while (mostRight.right != null && mostRight.right != cur) {
                   mostRight = mostRight.right;
               }
               if (mostRight.right == null) {
                   mostRight.right = cur;
                   cur = cur.left;
                   continue;
               } else {
                   mostRight.right = null;
               }
           }
           //只能到达1次的节点和能到达2次中的节点的第2次，会经过这里
           System.out.print(cur.value + " ");
           cur = cur.right;
       }
       System.out.println();
   }
   ```

   

4. Morris后序遍历