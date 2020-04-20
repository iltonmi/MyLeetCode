#  82. Remove Duplicates from Sorted List II 

双指针，常规操作，空间O(1)，时间O(N)

```java
public ListNode deleteDuplicates(ListNode head) {
    if (head == null || head.next == null) {
        return head;
    }

    // 2.哑节点,快慢指针
    ListNode dummy = new ListNode(-1);
    dummy.next = head;
    ListNode slow = dummy;
    ListNode fast = head;

    // 3.1 fast 遍历链表
    // 3.2 slow 变更：head 到 slow的链表,已经符合题目要求
    // 3.3 slow.next 变更：删除了重复元素
    while (fast != null)
    {
        if (fast.next == null || fast.val != fast.next.val)
        {                  
            if (slow.next == fast)      
            {
                slow = fast;
            }
            else                    
            {
                slow.next = fast.next;
            }
        }
        fast = fast.next;
    }

    return dummy.next;
}

public ListNode deleteDuplicates(ListNode head) {
    if (head == null || head.next == null) {
        return head;
    }
    ListNode dummy = new ListNode(0);
    dummy.next = head;
    ListNode slow = dummy;
    ListNode fast = head;
    while (fast != null) {
        if (fast.next != null && fast.val == fast.next.val) {
            while (fast.next != null && fast.val == fast.next.val) {
                fast = fast.next;
            }
            slow.next = fast.next;
            fast = fast.next;
        } else {
            slow = slow.next;
            fast = fast.next;
        }
    }
    return dummy.next;
}
```

