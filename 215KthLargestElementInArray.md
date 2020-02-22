#  215. Kth Largest Element in an Array

这题2条路：

1. 用小顶堆
2. 排序



1. 维护一个大小是K的小顶堆（既可以使用JDK的PriorityQueue，也可以手写堆），遍历数组，最后堆顶元素就是第K大的元素。时间O(NlogK)，空间O(logK)。

   1. 使用JDK的PriorityQueue实现：

      ```java
      class Solution {
          public int findKthLargest(int[] nums, int k) {
              PriorityQueue<Integer> pq = new PriorityQueue<>(k);
              for(Integer i : nums) {
                  pq.add(i);
                  if(pq.size() > k) {
                      pq.poll();
                  }
              }
              return pq.peek();
          }
      }
      ```

      

   2. 手写二叉堆实现：

      ```java
      class Solution {
          public int findKthLargest(int[] nums, int k) {
           FixedMinPQ<Integer> pq = new FixedMinPQ<>(k);
              for(Integer i : nums) {
                  if(pq.size() < k) {
                      pq.insert(i);
                  } else if(pq.size() == k && i > pq.peek()) {
                      pq.delMin();
                      pq.insert(i);
                  }
              }
              return pq.peek();
          }
          
          static class FixedMinPQ<E extends Comparable<E>> {
              E[] q;
              int size;
              
              public FixedMinPQ(int size) {
                  this.q = (E[]) new Comparable[size + 1];
              }
              
              public void insert(E key) {
                  q[++size] = key;
                  swim(size);
              }
              
              public E delMin() {
                  E min = q[1];
                  swap(1, size);
                  q[size--] = null;
                  sink(1);
                  return min;
              }
              
              private void swim(int k) {
                  while(k > 1 && less(k, k / 2)) {
                      swap(k, k / 2);
                      k /= 2;
                  }
              }
              
              private void sink(int k) {
                  while(2 * k <= size) {
                      int j = 2 * k;
                      if(j < size && less(j + 1, j)) {
                          j++;
                      }
                      if(!less(j, k)) {
                          break;
                      }
                      swap(k, j);
                      k = j;
                  }
              }
              
              private void swap(int i, int j) {
                  E t = q[i];
                  q[i] = q[j];
                  q[j] = t;
              }
              
              private boolean less(int i, int j) {
                  return q[i].compareTo(q[j]) < 0;
              }
              
              private int size() {
                  return this.size;
              }
              
              private E peek() {
                  return q[1];
              }
          }
      }
      ```
      
      

2. 快速选择

