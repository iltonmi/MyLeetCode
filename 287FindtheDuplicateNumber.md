#  287. Find the Duplicate Number

1. 散列表，空间时间O(N)。

2. 排序，然后遍历。复杂度视具体排序算法而定。

3. 将 nums[i] 视作一个节点，nums[i] 的值就是这个节点的next指针就会得到一个图。

   分析图结构分析。

   * 重复值代表的节点的入度大于1，其余节点入度是0或1

   * 考虑从图中移除重复节点和 nums[0] 的情况，称为图1：
     1. 剩余节点的入度是0或1，出度是1（可能指向null）：因此可能是多个链表
     2. 因为剩余节点入度最多是1，因此是无环链表

   * 考虑只从图中移除 nums[0] 的情况，称为图2：

     1. 对于每个无环链表，尾节点指向的下一个节点都不为null，因此只能是重复值代表的节点
     2. 每个节点的出度都是1，因此重复值节点必须指向其中一个无环链表的头节点或其自身，从而形成了环。
     3. 无论从图2的哪个节点出发，最终必然会从重复值代表的节点处进入环形链表。

   * 形容只移除了 nums[0] 的图结构：

     1. 重复节点在一个长度至少是1的环形链表之中，并且被多个无环链表的尾节点所指向。

     2. 通俗来说，重复节点被万箭穿心，并且在一个长度至少是1的环形链表之中。

   * 考虑全图：

     1. nums[0] 的入度是0，出度是1，只是某个无环链表的头节点。

   既然重复节点就是，环形链表的入口，那就可以利用 LinkedList2 中用到的 Floyd 算法。

   ```java
   class Solution {
       public int findDuplicate(int[] nums) {
        //fast, slow代表节点，其值等价于next指针
           int slow = nums[0];
           int fast = nums[nums[0]];
           while(fast != slow) {
               slow = nums[slow];
               fast = nums[nums[fast]];
           }
           //循环结束后，fast指向了相遇节点，而不在相遇节点
           //按照Floyd算法，fast应该从相遇节点出发，slow应该从头节点出发
           fast = nums[fast];
           slow = nums[0];
           while(slow != fast) {
               slow = nums[slow];
               fast = nums[fast];
           }
           return slow;
       }
   }
   ```
   
    