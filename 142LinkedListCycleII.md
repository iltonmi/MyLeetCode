#  142. Linked List Cycle II

 https://blog.csdn.net/To_be_to_thought/article/details/83958314 



1. Floyd算法（特定算法），时间O(N), 空间O(1)

   **算法步骤概述：**

   1. 使用快慢指针，若存在环，则2个指针一定会在环内相遇，记相遇在节点为A。
   2. 使用2个指针，1个指向头节点，一个指向A，同步出发，最终一定会在链表入环处第1个节点相遇。

   **模拟图如下：**

   ![](https://pic.leetcode-cn.com/99987d4e679fdfbcfd206a4429d9b076b46ad09bd2670f886703fb35ef130635-image.png)

   **理论证明：**

   > Just make a easier understanding.
> Suppose the first meet at step **k**,the distance between the start node of list and the start node of cycle is **F**, and the distance between the start node of cycle and the first meeting node is **a**. Then **2k = (F + a + n1(a+b)) = 2(F + a + n2(a+b)) ==> F + a = n(a+b) = nr.** Steps moving from start node to the start of the cycle is just **s**, moving from **m** by **s** steps would be the start of the cycle, covering n cycles. In other words, they meet at the entry of cycle.
   
**2k = (F + a + n1(a+b)) = 2(F + a + n2(a+b)) ==> F + a = n(a+b) = nr.**
   
   上面这条等式，说明：F + a = n 个环的长度。因此，从环的入口走 F + a 步会重新回到环的入口。
   
   即从A点（相遇点）走F步会回到环的入口。
   
   
   代码如下：
   
   ```java
   public class Solution {
       public ListNode detectCycle(ListNode head) {
           
           ListNode meetingNode = getMeetingNode(head);
           if(meetingNode == null) {
               return null;
           }
           ListNode node = head;
           while(node != meetingNode) {
               node = node.next;
               meetingNode = meetingNode.next;
           }
           return node;
       }
       
       private ListNode getMeetingNode(ListNode head) {
           ListNode slow = head;
           ListNode fast = head;
           while(fast != null && fast.next != null) {
               slow = slow.next;
               fast = fast.next.next;
               if(slow == fast) {
                   return fast;
               }
           }
           return null;
       }
   }
   ```
   
   
   
2. Set(正常人想到的)，时间O(N), 空间O(N)

   ```java
   public class Solution {
       Set<ListNode> set = new HashSet<>();
       public ListNode detectCycle(ListNode head) {
           while(head != null) {
               if(set.contains(head)) {
                   return head;
               }
               set.add(head);
               head = head.next;
           }
           return null;
       }
   }
   ```

   