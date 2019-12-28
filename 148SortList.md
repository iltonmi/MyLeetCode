#  148. Sort List

难度在于要求空间复杂度O(1)，时间复杂度O(NlogN)。

时间复杂度时O(NlogN)的排序算法：快排、归并、堆，**然而空间复杂度都不是O(1)**。

在leetcode的讨论区发现：**算法第4版里介绍了自底向上的归并排序，空间复杂度只有O(1)**

> 自顶向上的归并排序比较适合用链表组织的数据。这种方法只需要重新组织链表链接就能将链表原地排序（不需要创建任何新的链表节点）。

1. 自底向上归并排序，空间复杂度O(1)，时间复杂度证明：
   $$
   外循环次数log_2N
   $$
   内循环主要做2件事情：

   1. 将链表分成长度为size的子链表，需要遍历整个链表。
   2. 将 长度为size的子链表两两合并，需要遍历整个链表。

   $$
   内循环时间复杂度O(2N)=O(N)
   $$

   **总时间复杂度：**
   $$
   O(NlogN)
   $$
   

   ```java
   class Solution {
       public ListNode sortList(ListNode head) {
           ListNode dummy = new ListNode(0);
           dummy.next = head;
           int size = 0;
           while (head != null) {
               head = head.next;
               size++;
           }
           
           for (int step = 1; step < size; step <<= 1) {
               ListNode prev = dummy;
               ListNode cur = dummy.next;
               while(cur != null) {
                   ListNode left = cur;
                   ListNode right = split(left, step);
                   cur = split(right, step);
                   prev = merge(left, right, prev);
               }
           }
           
           return dummy.next;
       }
       
       /**
       * 以head为起点，分离出长度<=step的链表
       * 
       * @return 剩余部分链表的头节点
       **/
       private ListNode split(ListNode head, int step) {
           if(head == null) {
               return null;
           }
           
           for(int i = 0; i < step - 1 && head.next != null; i++) {
               head = head.next;
           }
           
           ListNode res = head.next;
           head.next = null;
           return res;
       }
       
       /**
       * 合并分别以left和right为头节点的2个链表
       * 
       * @return 合并后的链表的尾节点
       **/
       private ListNode merge(ListNode left, ListNode right, ListNode prev) {
           ListNode cur = prev;
            while(left != null && right != null) {
                if(left.val < right.val) {
                    cur.next = left;
                    left = left.next;
                } else {
                    cur.next = right;
                    right = right.next;
                }
                cur = cur.next;
            }
           
           if(left != null) {
               cur.next = left;
           } else if(right != null) {
               cur.next = right;
           }
           while(cur.next != null) {
               cur = cur.next;
           }
           return cur;
       }
   }
   ```

   

补充2种链表排序：

1. 自顶向下归并排序，空间复杂度O(logN)，时间复杂度O(NlogN)

   **写这道题的时候遇到一个问题：**出现了无限循环。

   **问题背景：**用快慢指针分段的时候，发现没有找到slow的前继节点。想着偷懒，令后半段链表的头节点为slow.next就可以少用1个指针。

   **引起问题的原因：**快慢指针的循环结束后只能保证slow非空，而不能保证slow.next非空，因此若以slow.next为后半段链表的头节点，可能会出现每次调用都将链表分为原链表和null，然后又对原链表进行递归调用，从而引起无限循环。

   **解决方法：**老老实实声明一个prevSlow变量，记录slow的前继节点。

   ```java
   class Solution {
       public ListNode sortList(ListNode head) {
           if(head == null || head.next == null) {
               return head;
           }
           ListNode prevSlow = null;
           ListNode slow = head;
           ListNode fast = head;
           while(fast != null && fast.next != null) {
               prevSlow = slow;
               slow = slow.next;
               fast = fast.next.next;
           }
           prevSlow.next = null;
           ListNode first = sortList(head);
           ListNode second = sortList(slow);
           
           return merge(first, second);
       }
       
       /**
       * 合并分别以left和right为头节点的2个链表
       * 
       * @return 合并后的链表的头
       **/
       private ListNode merge(ListNode left, ListNode right) {
           ListNode dummy = new ListNode(-1);
           ListNode cur = dummy;
            while(left != null && right != null) {
                if(left.val < right.val) {
                    cur.next = left;
                    left = left.next;
                } else {
                    cur.next = right;
                    right = right.next;
                }
                cur = cur.next;
            }
           
           if(left != null) {
               cur.next = left;
           } else if(right != null) {
               cur.next = right;
           }
           return dummy.next;
       }
   }
   ```

   

2. 选择排序，空间复杂度O(1)，时间复杂度O(N^2)

   ```java
   class Solution {
       public ListNode sortList(ListNode head) {
           ListNode tail = null;
           ListNode cur = head;
           ListNode smallPre = null;
           ListNode small = null;
           while(cur != null) {
               small = cur;
               smallPre = getPreOfSmallest(cur);
               //从未排序部分移除small
               if(smallPre != null) {
                   small = smallPre.next;
                   smallPre.next = small.next;
               }
               //若smallPre为null,则cur == small
               cur = cur == small ? cur.next : cur;
               if(tail == null) {
                   //用head保存排序部分
                   head = small;
               } else {
                   //链接small到排序部分
                   tail.next = small;
               }
               tail = small;
           }
           return head;
       }
       
       public ListNode getPreOfSmallest(ListNode head) {
           ListNode smallPre = null;
           ListNode small = head;
           ListNode pre = head;
           ListNode cur = head.next;
           while(cur != null) {
               if(cur.val < small.val) {
                   smallPre = pre;
                   small = cur;
               }
               pre = cur;
               cur = cur.next;
           }
           return smallPre;
       }
   }
   ```

   