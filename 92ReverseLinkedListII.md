#  92. Reverse Linked List II 

1. 迭代，空间O(1)，时间O(N)

   ```java
   public ListNode reverseBetween(ListNode head, int m, int n) {
       if(head.next == null) {
           return head;
       }
       ListNode dummy = new ListNode(Integer.MIN_VALUE);
       dummy.next = head;
       head = dummy;
       for(int i = 0; i < m - 1; ++i) {
           head = head.next;
       }
       ListNode reverseDummy = head;
       ListNode reverseTail = head.next;
       head = reverseTail.next;
       int diff = n - m;
       for(int i = diff; i > 0; --i) {
           reverseTail.next = head.next;
           head.next = reverseDummy.next;
           reverseDummy.next = head;
           head = reverseTail.next;
       }
       return dummy.next;
   }
   ```

   

2. 递归，空间O(N)，时间O(N)

    https://leetcode.com/articles/reverse-linked-list-ii/ 

   ```java
   class Solution {
   
       // Object level variables since we need the changes
       // to persist across recursive calls and Java is pass by value.
       private boolean stop;
       private ListNode left;
   
       public void recurseAndReverse(ListNode right, int m, int n) {
   
           // base case. Don't proceed any further
           if (n == 1) {
               return;
           }
   
           // Keep moving the right pointer one step forward until (n == 1)
           right = right.next;
   
           // Keep moving left pointer to the right until we reach the proper node
           // from where the reversal is to start.
           if (m > 1) {
               this.left = this.left.next;
           }
   
           // Recurse with m and n reduced.
           this.recurseAndReverse(right, m - 1, n - 1);
   
           // In case both the pointers cross each other or become equal, we
           // stop i.e. don't swap data any further. We are done reversing at this
           // point.
           if (this.left == right || right.next == this.left) {
               this.stop = true;            
           }
   
           // Until the boolean stop is false, swap data between the two pointers
           if (!this.stop) {
               int t = this.left.val;
               this.left.val = right.val;
               right.val = t;
   
               // Move left one step to the right.
               // The right pointer moves one step back via backtracking.
               this.left = this.left.next;
           }
       }
   
       public ListNode reverseBetween(ListNode head, int m, int n) {
           this.left = head;
           this.stop = false;
           this.recurseAndReverse(head, m, n);
           return head;
       }
   }
   ```

   