#  328. Odd Even Linked List 

1. 分链表，最后合并，注意边界条件。空间O(1)，时间O(N)。

   ```java
   public ListNode oddEvenList(ListNode head) {
       ListNode oddDummy = new ListNode(0);
       ListNode evenDummy = new ListNode(0);
       ListNode odd = oddDummy;
       ListNode even = evenDummy;
       boolean isOdd = true;
       while(head != null) {
           if(isOdd) {
               odd.next = head;
               head = head.next;
               odd = odd.next;
               odd.next = null;
           } else {
               even.next = head;
               head = head.next;
               even = even.next;
               even.next = null;
           }
           isOdd = !isOdd;
       }
       if(odd != null) {
           odd.next = evenDummy.next;
       }
       return oddDummy.next;
   }
   ```

   