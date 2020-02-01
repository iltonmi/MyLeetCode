#  406. Queue Reconstruction by Height

 https://leetcode-cn.com/problems/queue-reconstruction-by-height/solution/gen-ju-shen-gao-zhong-jian-dui-lie-by-leetcode/ 

1. 贪心，空间O(N)，时间O(max(N^2, 排序时间复杂度))。

   每次插入到输出队列的时间复杂度时O(K)，K从1到N-1，因此插入的时间复杂度时O(N ^ 2)。

   **策略：**

   >  让我们从最简单的情况下思考，当队列中所有人的 `(h,k)` 都是相同的高度 `h`，只有 `k` 不同时，解决方案很简单：每个人在队列的索引 `index = k`。 
   >
   >  即使不是所有人都是同一高度，这个策略也是可行的。因为个子矮的人相对于个子高的人是 “看不见” 的，所以可以先安排个子高的人。 

   **算法：**

   > 该策略可以递归进行：
   >
   > * 将最高的人按照 k 值升序排序，然后将它们放置到输出队列中与 k 值相等的索引位置上。
   > * 按降序取下一个高度，同样按 k 值对该身高的人升序排序，然后逐个插入到输出队列中与 k 值相等的索引位置上。
   > * 直到完成为止。

   ```java
   class Solution {
     public int[][] reconstructQueue(int[][] people) {
       Arrays.sort(people, (o1, o2) 
                   -> o1[0] == o2[0] ? o1[1] - o2[1] : o2[0] - o1[0]);
   
       List<int[]> output = new LinkedList<>();
       for(int[] p : people){
         output.add(p[1], p);
       }
   
       return output.toArray(new int[people.length][2]);
     }
   }
   ```

   