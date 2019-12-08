#  19. Remove Nth Node From End of List 

1. 快慢指针，哑节点，时间O(n)，空间O(1)，遍历一次即可

   **哑节点的作用**

   1. 当需要移除第k个节点，必然需要连接第k-1个节点和第k+1个节点，即前继节点和后继节点。

   2. 首先看后继个节点，这个节点即使是null也没问题，因为后继节点为null是有效值
   3. 接着看第k-1个节点，若被移除节点为链表的头节点，头节点的前继节点必然是null，使用null连接后继节点必然会引起NullPointerException。
   4. 因此，主动为头节点构造一个前继节点，即哑节点，即可避免抛出异常。

   ```java
   /**
    * Definition for singly-linked list.
    * public class ListNode {
    *     int val;
    *     ListNode next;
    *     ListNode(int x) { val = x; }
    * }
    */
   class Solution {
       public ListNode removeNthFromEnd(ListNode head, int n) {
           ListNode dummy = new ListNode(Integer.MIN_VALUE);
           dummy.next = head;
           ListNode slow = dummy;
           ListNode fast = dummy;
           for(int i = 0; i < n + 1; i++) {
               fast = fast.next;
           }
           while(fast != null) {
               fast = fast.next;
               slow = slow.next;
           }
           slow.next = slow.next.next;
           return dummy.next;
       }
   }
   ```

   