#  116. Populating Next Right Pointers in Each Node 

 https://leetcode-cn.com/problems/populating-next-right-pointers-in-each-node/solution/tian-chong-mei-ge-jie-dian-de-xia-yi-ge-you-ce-j-3/ 

1. 辅助队列，层次遍历，空间O(N)，时间O(N)

   ```java
   public Node connect(Node root) {
           
       if (root == null) {
           return root;
       }
   
       // Initialize a queue data structure which contains
       // just the root of the tree
       Deque<Node> Q = new LinkedList<Node>(); 
       Q.add(root);
   
       // Outer while loop which iterates over 
       // each level
       while (Q.size() > 0) {
   
           // Note the size of the queue
           int size = Q.size();
   
           // Iterate over all the nodes on the current level
           for(int i = 0; i < size; i++) {
   
               // Pop a node from the front of the queue
               Node node = Q.poll();
   
               // This check is important. We don't want to
               // establish any wrong connections. The queue will
               // contain nodes from 2 levels at most at any
               // point in time. This check ensures we only 
               // don't establish next pointers beyond the end
               // of a level
               if (i < size - 1) {
                   node.next = Q.peek();
               }
   
               // Add the children, if any, to the back of
               // the queue
               if (node.left != null) {
                   Q.add(node.left);
               }
               if (node.right != null) {
                   Q.add(node.right);
               }
           }
       }
   
       // Since the tree has now been modified, return the root node
       return root;
   }
   ```

   

2. 利用上一层的next指针，空间O(1)，时间O(N)

   ```java
   public Node connect(Node root) {
           
       if (root == null) {
           return root;
       }
   
       // Start with the root node. There are no next pointers
       // that need to be set up on the first level
       Node leftmost = root;
   
       // Once we reach the final level, we are done
       while (leftmost.left != null) {
   
           // Iterate the "linked list" starting from the head
           // node and using the next pointers, establish the 
           // corresponding links for the next level
           Node head = leftmost;
   
           while (head != null) {
   
               // CONNECTION 1
               head.left.next = head.right;
   
               // CONNECTION 2
               if (head.next != null) {
                   head.right.next = head.next.left;
               }
   
               // Progress along the list (nodes on the current level)
               head = head.next;
           }
   
           // Move onto the next level
           leftmost = leftmost.left;
       }
   
       return root;
   }
   ```

   