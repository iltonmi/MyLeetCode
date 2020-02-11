#  234. Palindrome Linked List

 https://leetcode-cn.com/problems/palindrome-linked-list/solution/hui-wen-lian-biao-by-leetcode/ 

1. （修改链表）快慢指针找到中间节点，翻转后半部分链表，遍历对比，还原链表。空间O(1)，时间O(N)。

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

2. 使用列表储存节点的值，保存前继节点的指针，双指针检查。空间O(N)，时间O(N)。

   ```java
   public boolean isPalindrome(ListNode head) {
       List<Integer> vals = new ArrayList<>();
   
       // Convert LinkedList into ArrayList.
       ListNode currentNode = head;
       while (currentNode != null) {
           vals.add(currentNode.val);
           currentNode = currentNode.next;
       }
   
       // Use two-pointer technique to check for palindrome.
       int front = 0;
       int back = vals.size() - 1;
       while (front < back) {
           // Note that we must use !equals() instead of !=
           // because we are comparing Integer, not int.
           if (!vals.get(front).equals(vals.get(back))) {
               return false;
           }
           front++;
           back--;
       }
       return true;
   }
   ```

   

3. 栈或递归栈，保存前继节点的指针(太挫了)。空间O(N)，时间O(N)。

   1. 递归，其实有冗余。

      ```java
      class Solution {
      
          private ListNode frontPointer;
      
          private boolean recursivelyCheck(ListNode currentNode) {
              if (currentNode != null) {
                  if (!recursivelyCheck(currentNode.next)) {
                      return false;
                  }
                  if (currentNode.val != frontPointer.val) {
                      return false;
                  }
                  frontPointer = frontPointer.next;
              }
              return true;
          }
      
          public boolean isPalindrome(ListNode head) {
              frontPointer = head;
              return recursivelyCheck(head);
          }
      }
      ```

      

   2. 栈，直接把冗余的扔了。

      ```java
      public boolean isPalindrome(ListNode head) {
          if(head == null) {
              return true;
          }
          Deque<ListNode> stk = new ArrayDeque<>();
          ListNode front = head;
          while(head != null) {
              stk.addFirst(head);
              head = head.next;
          }
          while(front != stk.peekFirst() && stk.peekFirst().next != front) {
              if(front.val != stk.peekFirst().val) {
                  return false;
              }
              front = front.next;
              stk.removeFirst();
          }
          return true;
      }
      ```

      