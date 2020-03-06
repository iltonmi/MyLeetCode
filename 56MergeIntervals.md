#  56. Merge Intervals 

1. 排序，空间O(N)，时间O(N)。

   下面的代码修改了原数组。

   ```java
   public int[][] merge(int[][] intervals) {
       if (intervals.length <= 1) {
           return intervals;
       }
   
       // Sort by ascending starting point
       Arrays.sort(intervals, (i1, i2) -> Integer.compare(i1[0], i2[0]));
   
       List<int[]> result = new ArrayList<>();
       int[] newInterval = intervals[0];
       //先加入，后更新
       result.add(newInterval);
       for (int[] interval : intervals) {
           if (interval[0] <= newInterval[1]) // Overlapping intervals, move the end if needed
               newInterval[1] = Math.max(newInterval[1], interval[1]);
           else {                             // Disjoint intervals, add the new interval to the list
               newInterval = interval;
               //先加入，后更新
               result.add(newInterval);
           }
       }
   
       return result.toArray(new int[result.size()][]);
   }
   ```

   