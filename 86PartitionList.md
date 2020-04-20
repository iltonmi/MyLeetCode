#  86. Partition List

双指针，分别保留大于x的部分和小于x的部分，常规操作，空间O(1)，时间O(N)

```java
public ListNode partition(ListNode head, int x) {
    ListNode sDummy = new ListNode(Integer.MIN_VALUE);
    ListNode lDummy = new ListNode(Integer.MIN_VALUE);
    ListNode small = sDummy;
    ListNode large = lDummy;
    while(head != null) {
        if(head.val < x) {
            small.next = head;
            head = head.next;
            small = small.next;
            small.next = null;
        }
        else
        {
            large.next = head;
            head = head.next;
            large = large.next;
            large.next = null;
        }
    }
    small.next = lDummy.next;
    return sDummy.next;
}
```



```java
public ListNode partition(ListNode head, int x) {
    if(head == null) {
        return null;
    }
    //保存大于或等于x的节点
    ListNode dummy1 = new ListNode(0);
    dummy1.next = head;
    //保存小于x的节点
    ListNode dummy2 = new ListNode(0);
    ListNode cur = dummy1;
    ListNode pos = dummy2;
    while (cur.next != null) {
        if (cur.next.val < x) {
            pos.next = cur.next;
            cur.next = cur.next.next;
            pos = pos.next;
        } else {
            cur = cur.next;
        }
    }
    pos.next = dummy1.next;
    return dummy2.next;
}
```

