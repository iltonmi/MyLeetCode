#  2. Add Two Numbers

这只是数学题。。。。

1. 先处理相等长度部分，再处理多余部分。时间O(n)，空间O(1)。

   ```java
   /**
    * Definition for singly-linked list.
    * public class ListNode {
    *     int val;
    *     ListNode next;
    *     ListNode(int x) { val = x; }
    * }
    */
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
       if(l1 == null) {
           return l2;
       }
       if(l2 == null) {
           return l1;
       }
       int carry = 0;
       ListNode dummy = new ListNode(Integer.MIN_VALUE);
       ListNode cur = dummy;
       while(l1 != null && l2 != null) {
           int val1 = l1.val;
           int val2 = l2.val;
           int sum = val1 + val2 + carry;
           carry = sum / 10;
           cur.next = new ListNode(sum % 10);
           cur = cur.next;
           l1 = l1.next;
           l2 = l2.next;
       }
       ListNode longerPart = l1 == null ? l2 : l1;
       while(longerPart != null) {
           int sum = carry + longerPart.val;
           carry = sum / 10;
           cur.next = new ListNode(sum % 10);
           cur = cur.next;
           longerPart = longerPart.next;
       }
       if(carry == 1) {
           cur.next = new ListNode(1);
       }
       return dummy.next;
   }
   ```

   

2. 在一个循环中处理

   ```java
   public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
       if(l1 == null) {
           return l2;
       }
       if(l2 == null) {
           return l1;
       }
       int carry = 0;
       ListNode dummy = new ListNode(Integer.MIN_VALUE);
       ListNode cur = dummy;
       while(l1 != null || l2 != null) {
           int sum = carry;
           if(l1 != null) {
               sum += l1.val;
               l1 = l1.next;
           }
           if(l2 != null) {
               sum += l2.val;
               l2 = l2.next;
           }
           carry = sum / 10;
           cur.next = new ListNode(sum % 10);
           cur = cur.next;
       }
       if(carry == 1) {
           cur.next = new ListNode(1);
       }
       return dummy.next;
   }
   ```

   