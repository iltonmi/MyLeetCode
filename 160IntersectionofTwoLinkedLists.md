#  160. Intersection of Two Linked Lists

1. 遍历计算两个链表的长度差，空间O(1)，时间O(N)

   ```java
   /**
    * Definition for singly-linked list.
    * public class ListNode {
    *     int val;
    *     ListNode next;
    *     ListNode(int x) {
    *         val = x;
    *         next = null;
    *     }
    * }
    */
   public class Solution {
       public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
           if(headA == null || headB == null) {
               return null;
           }
           int count1 = 0;
           int count2 = 0;
           ListNode cur1 = headA;
           ListNode cur2 = headB;
           //迭代过后，cur1和cur2分别是headA和headB为头节点的链表的尾节点
           //迭代过后，count1和count2分别是headA和headB为头节点的链表长度值减1
           while(cur1.next != null || cur2.next != null) {
               if(cur1.next != null) {
                   cur1 = cur1.next;
                   count1++;
               }
               if(cur2.next != null) {
                   cur2 = cur2.next;
                   count2++;
               }
           }
           //若cur1 != cur2, 则两个链表没有相交
           if(cur1 != cur2) {
               return null;
           }
           //到此位置，说明两个链表必定相交
           //对齐头节点，使得headA和headB到相交节点的距离相等
           if(count1 > count2) {
               for(int i = count1 - count2; i > 0; i--) {
                   headA = headA.next;
               }
           } else if (count2 > count1) {
               for(int i = count2 - count1; i > 0; i--) {
                   headB = headB.next;
               }
           }
           //对齐头节点后，同时迭代headA和headB,必定会在其中一次迭代同时指向相交节点
           while(headA != headB) {
               headA = headA.next;
               headB = headB.next;
           }
           return headA;
       }
   }
   ```

   

2. 交换路径，时间O(n), 空间O(1)

   思路：

   1. 假设headA到相交节点的路径为路径A，headB到相交节点的路径为路径B
   2.  第一次遍历到尽头后交换路径，相当于A的路径为：路径A+相交路径+路径B，而B的路径为：路径B+相交路径+路径A
   3. 路径A或路径B之后就是相交节点，headA和headB会相遇
   4. 如果没有相交节点，就相当于他们的相交节点为null，交换路径后也会同时达到null

   ```java
       public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
           ListNode fhA = headA, fhB = headB;
           //针对同时null的情况，会直接相等
           while(headA != headB) {
              	//当第一次遍历完链表时，交换路径
               headA = headA == null ? fhB : headA.next;
               headB = headB == null ? fhA : headB.next;
           }
           return headA;
       }
   ```

   

3. hash table, 时间O(m+n),  空间O(m)或O(n)

   ```java
       public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
           Set<ListNode> set = new HashSet<>();
           while(headA != null) {
               set.add(headA);
               headA = headA.next;
           }
           while(headB != null) {
               if(set.contains(headB)) {
                   return headB;
               }
               headB = headB.next;
           }
           return null;
       }
   ```

   

4. 暴力, 时间O(mn),  空间O(1)

   ```java
       public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
           ListNode curB;
           while(headA != null) {
               curB = headB;
               while(curB != null) {
                   if(curB == headA) {
                       return headA;
                   }
                   curB = curB.next;
               }
               headA = headA.next;
           }
           return null;
       }
   ```

   