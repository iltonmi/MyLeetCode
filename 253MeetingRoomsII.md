# 253. Meeting Rooms II

 https://leetcode-cn.com/problems/meeting-rooms-ii/solution/hui-yi-shi-ii-by-leetcode/ 

1. 排序，小顶堆，空间O(N)，时间O(NlogN)。

   小顶堆保存会议的结束时间，堆顶元素代表最早空出来的会议室。

   遍历所有会议，将会议的开始时间和堆顶元素比较，查看是否有会议已经结束：

   1. 有的话，移除已经结束的会议，插入当前会议的结束时间，这代表着新的会议占用结束的会议的会议室。

   2. 没有的话，代表没有空闲会议室，直接插入当前会议的结束时间，这代表着占用新的会议室

   小顶堆的数量代表已经开启的会议室数量。

   ```java
   public int minMeetingRooms(int[][] intervals) {
   
       // Check for the base case. If there are no intervals, return 0
       if (intervals.length == 0) {
         return 0;
       }
   
       // Min heap
       PriorityQueue<Integer> allocator =
           new PriorityQueue<Integer>(intervals.length, (a, b) -> a - b);
   
       // Sort the intervals by start time
       Arrays.sort(intervals, (a, b) -> a[0] - b[0]);
   
       // Add the first meeting
       allocator.add(intervals[0][1]);
   
       // Iterate over remaining intervals
       for (int i = 1; i < intervals.length; i++) {
   
         // If the room due to free up the earliest is free, assign that room to this meeting.
         if (intervals[i][0] >= allocator.peek()) {
           allocator.poll();
         }
   
         // If a new room is to be assigned, then also we add to the heap,
         // If an old room is allocated, then also we have to add to the heap with updated end time.
         allocator.add(intervals[i][1]);
       }
   
       // The size of the heap tells us the minimum rooms required for all the meetings.
       return allocator.size();
   }
   ```

   

2. 开始时间和结束时间分离，空间O(N)，时间O(NlogN)。排序占用时间了emmm。

   >  当我们遇到“会议结束”事件时，意味着一些较早开始的会议已经结束。我们并不关心到底是哪个会议结束。我们所需要的只是 **一些** 会议结束,从而提供一个空房间。 

   不变量：某个会议结束了，它必定已经开始了。

   ```java
   public int minMeetingRooms(int[][] intervals) {
   
      // Check for the base case. If there are no intervals, return 0
       if (intervals.length == 0) {
         return 0;
       }
   
       Integer[] start = new Integer[intervals.length];
       Integer[] end = new Integer[intervals.length];
   
       for (int i = 0; i < intervals.length; i++) {
         start[i] = intervals[i][0];
         end[i] = intervals[i][1];
       }
   
       // Sort the intervals by end time
       Arrays.sort(end, (a, b) -> a - b);
   
       // Sort the intervals by start time
       Arrays.sort(start, (a, b) -> a - b);
   
       // The two pointers in the algorithm: e_ptr and s_ptr.
       int startPointer = 0;
       int endPointer = 0;
   
       // Variables to keep track of maximum number of rooms used.
       int usedRooms = 0;
   
       // Iterate over intervals.
       while (startPointer < intervals.length) {
         //不变量：某个会议结束了，它必定已经开始了
         if (start[startPointer] >= end[endPointer]) {
           //占用空闲房间
           endPointer += 1;
         } else {
           //开启新房间
           usedRooms += 1;
         }
         startPointer += 1;
       }
       return usedRooms;
   }
   ```

   