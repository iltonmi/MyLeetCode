#  141. Linked List Cycle

1. 快慢指针，时间O(n), 空间O(1)

   ```java
   /**
    * Definition for singly-linked list.
    * class ListNode {
    *     int val;
    *     ListNode next;
    *     ListNode(int x) {
    *         val = x;
    *         next = null;
    *     }
    * }
    */
   public class Solution {
       public boolean hasCycle(ListNode head) {
           if(head == null || head.next == null) {
               return false;
           }
           ListNode slow = head;
           ListNode fast = head.next;
           while(fast != null && fast.next != null) {
               slow = slow.next;
               fast = fast.next.next;
               if(slow == fast) {
                   return true;
               }
           }
           return false;
       }
   }
   ```

   

2. 哈希表, 时间O(n), 空间O(n)

   ```java
   public class Solution {
       public boolean hasCycle(ListNode head) {
           if(head == null) {
               return false;
           }
           Set<ListNode> set = new HashSet<>();
           while(head != null) {
               if(set.contains(head)) {
                   return true;
               }
               set.add(head);
               head = head.next;
           }
           return false;
       }
   }
   ```

   