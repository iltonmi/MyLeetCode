#  138. Copy List with Random Pointer 

这道题的要求是复制链表，难点在于设置random指针。

定义：

```java
/*
// Definition for a Node.
class Node {
    int val;
    Node next;
    Node random;

    public Node(int val) {
        this.val = val;
        this.next = null;
        this.random = null;
    }
}
*/
```



**从最基础的想法出发**，假如这是一个只有next指针的链表，那么我们会遍历一次原链表就够了。

引入了random指针之后，这样做就行不通了，因为当我们设置random指针时，random所指向的节点未必存在。

**2种方法：**

1. 第一次遍历时设置next指针而不设置random指针，第二次遍历时才设置random指针。
2. 只遍历一次，若设置random指针时节点不存在，则创建random指向的节点。这意味着，在第一次遍历时，某些节点由于被前面某些节点的random指针所指向，已经被提前创建了。对于这些节点，无需重新创建，直接设置next指针和random指针即可（如果重新创建，又需要重新遍历，找到random指针所在节点并重新设置random指针，时间复杂度太高）。

**可行性分析：**

1. 对于第一种做法：由于random指针指向的是节点而不是索引，因此在第2次遍历时，必须能通过原节点找到对应的新节点才能满足要求。
2. 对于第二种做法：对于因为被random指针指向而创建的节点，我们需要在遍历到其原节点时将其找到。因此，也需要能够通过原节点早到新节点。

**总结：**2种方法都需要能够通过原节点找到新的节点。

**具体实现：**

1. 第2种方法实现：散列映射，oldNode - > newNode。时间O(N), 空间O(N)

   ```java
   class Solution {
       Map<Node, Node> map = new HashMap<>();
       public Node copyRandomList(Node head) {
           //辅助节点，无意义
           Node preNewNode = new Node(-1);
           Node cur = head;
           while(cur != null) {
               Node node = map.get(cur);
               //判断对应新节点是否已经被创建
               if(node == null) {
                   node = new Node(cur.val);
                   map.put(cur, node);
               }
               preNewNode.next = node;
               //判断是否存在random节点
               if(cur.random != null) {
                   Node random = map.get(cur.random);
                   //判断random节点是否已经被创建
                   if(random != null) {
                       node.random = random;
                   } else {
                       node.random = new Node(cur.random.val);
                       map.put(cur.random, node.random);
                   }
               }
               preNewNode = node;
               cur = cur.next;
           }
           return map.get(head);
       }
   }
   ```

   

   1. 第一种方法实现：（副作用：修改原链表结构）。第一次遍历，将新节点保存在原链表中原节点的后一个位置。第二次遍历，设置random指针，此时还不能够将新节点从原节点去除，否则设置random指针时可能无法找到新节点。时间O(N), 空间O(1)

   ```java
   class Solution {
       public Node copyRandomList(Node head) {
           if(head == null) {
               return null;
           }
           Node cur = head;
           //创建新节点，保存在原节点的后一个节点
           while(cur != null) {
               Node newNode = new Node(cur.val);
               newNode.next = cur.next;
               cur.next = newNode;
               cur = cur.next.next;
           }
           cur = head;
           //设置新节点的random指针
           while(cur != null) {
               if(cur.random != null) {
                   cur.next.random = cur.random.next;
               }
               cur = cur.next.next;
           }
           cur = head;
           Node dummy = new Node(-1);
           Node curNew = dummy;
           //从原链表中取出新节点，恢复原链表结构
           while(cur != null) {
               //更新curNew
               curNew.next = cur.next;
               //从原节点去除新节点
               curNew = curNew.next;
               cur.next = curNew.next;
               curNew.next = null;
               
               cur = cur.next;
           }
           return dummy.next;
       }
   }
   ```

   