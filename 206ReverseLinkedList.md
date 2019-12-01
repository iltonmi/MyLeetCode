#  206. Reverse Linked List

1. 迭代，时间O(n), 空间O(1)

   ```java
   /**
    * Definition for singly-linked list.
    * public class ListNode {
    *     int val;
    *     ListNode next;
    *     ListNode(int x) { val = x; }
    * }
    */
    public ListNode reverseList(ListNode head) {
       ListNode reverHead = null;
       while(head != null) {
           ListNode tempNext = head.next;
           head.next = reverHead;
           reverHead = head;
           head = tempNext;
       }
       return reverHead;
   }
   ```

   

2. 递归，时间O(n), 空间O(n)

   ```java
   public ListNode reverseList(ListNode head) {
           if(head == null || head.next == null) {
               return head;
           }
           ListNode p = reverseList(head.next);
           head.next.next = head;
           head.next = null;
           return p;
       }
   ```

   