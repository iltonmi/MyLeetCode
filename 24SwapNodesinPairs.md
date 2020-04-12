#  24. Swap Nodes in Pairs 

1. 迭代，空间O(1)，时间O(N)

   ```java
   public ListNode swapPairs(ListNode head) {
           
       ListNode dummy = new ListNode(0);
       dummy.next = head;
   
       ListNode pre = dummy;
   
       while(head != null && head.next != null) {
   
           ListNode one = head;
           ListNode two = head.next;
   
           pre.next = two;
           one.next = two.next;
           two.next = one;
   
           head = one.next;
           pre = one;
       }
       return dummy.next;
   }
   ```

   

2. 递归，空间O(N)，时间O(N)

   ```java
   public ListNode swapPairs(ListNode head) {
       if(head == null || head.next == null) {
           return head;
       }
   
       ListNode one = head;
       ListNode two = head.next;
   
       one.next = swapPairs(two.next);
       two.next = one;
       return two;
   }
   ```

   