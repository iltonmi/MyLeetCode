# 203. Remove Linked List Elements

集邮。

```java
public ListNode removeElements(ListNode head, int val) {
    ListNode dummy = new ListNode(-1);
    dummy.next = head;
    head = dummy;
    while(head.next != null) {
        if(head.next.val == val) {
            head.next = head.next.next;
        } else {
            head = head.next;
        }
    }
    return dummy.next;
}
```

