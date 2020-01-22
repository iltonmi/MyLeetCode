#  215. Kth Largest Element in an Array

这题2条路：

1. 用小顶堆
2. 排序



1. 维护一个大小是K的小顶堆（既可以使用JDK的PriorityQueue，也可以手写堆），遍历数组，最后堆顶元素就是第K大的元素。时间O(NlogK)，空间O(logK)。

   

2. 排序后取第K个元素

