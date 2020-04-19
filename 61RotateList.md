#  61. Rotate List 

链表的题目，重点就是要考虑清楚至少需要哪些指针来完成，怎么样能最快的获取需要的指针并且完成操作。

这一题，如果要rotate right，需要分割点的指针，还有尾节点的指针。

空间O(1)，时间O(N)。

```java
public ListNode rotateRight(ListNode head, int k) {
    if(head == null) {
        return null;
    }
    int size = 1;
    ListNode tail = head;
    while(tail.next != null){
        tail = tail.next;
        ++size;
    }
    //如果size == 1，那么无论k的值，rotate的结果都是head
    if(size == 1) {
        return head;
    }
    k %= size;
    if(k == 0) {
        return head;
    }
    int step = size - k;
    ListNode cur = head;
    for(int i = 1; i < step; ++i){
        cur = cur.next;
    }
    tail.next = head;
    head = cur.next;
    cur.next = null;
    return head;
}
```

