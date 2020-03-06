#  57. Insert Interval 

1. 贪心，合并的过程参考56 Merge Intervals

   1.  在区间 `newInterval` 之前开始的区间全部添加到输出中。 
   2.  将 `newInterval` 添加到输出中，如果与输出中的最后一个区间重合则合并它们。 
   3.  然后一个个添加后续的区间，如果重合则合并它们。

   ```java
   public int[][] insert(int[][] intervals, int[] newInterval) {
       int idx = 0;
       int n = intervals.length;
       LinkedList<int[]> output = new LinkedList<>();
   
       int[] preInterval = null;
       // add all intervals starting before newInterval
       while (idx < n && intervals[idx][0] < newInterval[0]) {
           int[] temp = intervals[idx++];
           preInterval = temp;
           output.add(temp);
       }
   
       // add newInterval
       // if there is no overlap, just add the interval
       if (preInterval == null || preInterval[1] < newInterval[0]) {
           preInterval = newInterval;
           output.add(preInterval);
       } else {
           preInterval[1] = Math.max(preInterval[1], newInterval[1]);
       }
   
       // add next intervals, merge with newInterval if needed
       for(; idx < n; idx++) {
           int[] curInterval = intervals[idx];
           if(curInterval[0] <= preInterval[1]) {
               preInterval[1] = Math.max(preInterval[1], curInterval[1]);
           } else {
               preInterval = curInterval;
               output.add(curInterval);
           }
       }
       return output.toArray(new int[output.size()][2]);
   }
   ```

   

