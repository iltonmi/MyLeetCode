#  237. Delete Node in a Linked List

注意题目，被删节点不是尾节点。

```java
public void deleteNode(ListNode node) {
    node.val = node.next.val;
    node.next = node.next.next;
}
```

