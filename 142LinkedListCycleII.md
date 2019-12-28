#  142. Linked List Cycle II

 https://blog.csdn.net/To_be_to_thought/article/details/83958314 



1. Floyd算法（特定算法），时间O(N), 空间O(1)

   **算法步骤概述：**

   1. 使用快慢指针，若存在环，则2个指针一定会在环内相遇，记相遇在节点为A。
   2. 使用2个指针，1个指向头节点，一个指向A，同步出发，最终一定会在链表入环处第1个节点相遇。

   **模拟图如下：**

   ![](https://pic.leetcode-cn.com/99987d4e679fdfbcfd206a4429d9b076b46ad09bd2670f886703fb35ef130635-image.png)

   **理论证明：**

   1. 看图说话：假设环的长度是C，头节点到节点0（入口节点）的部分为F。快节点位于节点h的位置。

   2. 在某一时刻，快慢指针在节点h相遇。

   3. 有如下公式
      $$
      慢指针经过节点数S_1=F+a+c_1(a+b)
      $$
   
      $$
      快指针经过节点数S_2=F+a+c_2(a+b)
      $$
   
      由快慢指针的倍速关系可以得到如下方程：
      $$
      2S_1=S_2,代入得到：F+a=(c_2-2c_1)(a+b),进一步得到,F=(c_2-2c_1-1)(a+b)+b
      $$
   
      $$
      c_1=\frac{c_2}{2}+\frac{b-2F-a}{2(a+b)}
      $$
   
      无论c1和c2如何变化，F的值都不会变，因此:
      $$
      令c_2-2c_1-1=0,得到F=b
      $$
   
    **总结：当环存在的时候，链表有一个性质，就是头结点到入环点和快慢指针碰撞点到入环点的距离相等。** 
   
   **时间复杂度证明：**
   
   ​	1. 假设经过F次迭代后，慢节点到达入环的第1个节点，此时快节点位于h的位置。
   
    2. 后来经过x次迭代，快慢节点相遇。得到公式如下：
       $$
       2x=x+a+(a+b),x=2a+b
       $$
   
    3. 因此，时间复杂度如下：
       $$
       F+x=F+(a+b)+a=N+a=O(N)
       $$
   
   **补充一个问题：为什么快慢指针迭代步数的倍数是2？**
   

   
   
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

   