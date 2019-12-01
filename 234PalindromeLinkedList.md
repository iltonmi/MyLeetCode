#  234. Palindrome Linked List

1. 快慢指针找到中间节点，翻转后半部分链表，遍历对比，还原链表。时间O(n), 空间O(1)

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
       public boolean isPalindrome(ListNode head) {
           if(head == null || head.next == null) {
               return true;
           }
           ListNode slow = head;
           ListNode fast = head.next;
           //若节点数为奇数，则slow指向中间节点
           //若节点数为偶数，则slow指向中间两个节点的前者
           while(fast != null && fast.next != null) {
               slow = slow.next;
               fast = fast.next.next;
           }
           //翻转后半部分链表
           ListNode cur = reverseList(slow.next);
           slow.next = cur;
           while(cur != null) {
               if(cur.val != head.val) {
                   //还原链表
                   slow.next = reverseList(slow.next);
                   return false;
               }
               cur = cur.next;
               head = head.next;
           }
           //还原链表
           slow.next = reverseList(slow.next);
           return true;
       }
       
       private ListNode reverseList(ListNode head) {
           ListNode reverHead = null;
           while(head != null) {
               ListNode tempNext = head.next;
               head.next = reverHead;
               reverHead = head;
               head = tempNext;
           }
           return reverHead;
       }
   }
   ```

   