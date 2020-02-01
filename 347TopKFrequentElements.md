#  347. Top K Frequent Elements 

1. 散列表和二叉堆，空间O(N)，时间O(NlogN)

   ```java
   class Solution {
       private Map<Integer, Integer> map = new HashMap<>();
       
       public List<Integer> topKFrequent(int[] nums, int k) {
           for(int num : nums) {
               map.put(num, map.getOrDefault(num, 0) + 1);
           }
           
           MinPQ<Integer> pq = new MinPQ<>(k, (e1, e2) 
                                           -> map.get(e1) - map.get(e2));
           
           for(int n : map.keySet()) {
               pq.insert(n);
               if(pq.size() > k) {
                   pq.delMin();
               }
           }
           
           List<Integer> topK = new LinkedList<>();
           while(pq.size() > 0) {
               topK.add(pq.delMin());
           }
           return topK;
       }
       
       class MinPQ<E> {
           
           private Object[] q;
           private Comparator<? super E> cmp;
           private int size;
           
           public MinPQ(int maxSize, Comparator<? super E> cmp) {
               q = new Object[maxSize+2];
               this.cmp = cmp;
           }
           
           public void insert(E e) {
               q[++size] = e;
               swim(size);
           }
           
           public E delMin() {
               Object minVal = q[1];
               swap(1, size);
               q[size] = null;
               size--;
               sink(1);
               return (E) minVal;
           }
           
           public E peek() {
               return (E)q[1];
           }
           
           private void swim(int k) {
               while(k > 1 && larger(k / 2, k)) {
                   swap(k / 2, k);
                   k /= 2;
               }
           }
           
           private void sink(int k) {
               while(2 * k <= size) {
                   int j = 2 * k;
                   if(j < size && larger(j, j + 1)) {
                       j++;
                   }
                   if(!larger(k, j)) {
                       break;
                   }
                   swap(k, j);
                   k = j;
               }
           }
           
           private boolean larger(int i, int j) {
               return cmp.compare((E) q[i], (E) q[j]) > 0;
           }
           
           private void swap(int i, int j) {
               Object temp = q[i];
               q[i] = q[j];
               q[j] = temp;
           }
           
           private int size() {
               return this.size;
           }
       }
   }
   ```

   