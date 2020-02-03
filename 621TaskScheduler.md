#  621. Task Scheduler

 https://leetcode-cn.com/problems/task-scheduler/solution/ren-wu-diao-du-qi-by-leetcode/ 

其实方法1的贪心策略无法直接证明是对的，**只能方法2的思路证明方法1的贪心策略**。

1. 排序或优先级队列，时间O(time)

   无论是排序还是优先级队列，都是为了先分配出现次数最多的任务。

   结论：每轮最多做N+1个任务，先做出现次数最多的任务。

   证明：出现次数最多的任务不参与的轮次，相当于方法2中空闲时间被填满后剩余的任务。

   * 排序

     ```java
     public int leastInterval(char[] tasks, int n) {
         int[] map = new int[26];
         for (char c: tasks)
             map[c - 'A']++;
         Arrays.sort(map);
         int time = 0;
         while (map[25] > 0) {
             int i = 0;
             while (i <= n) {
                 if (map[25] == 0)
                     break;
                 if (i < 26 && map[25 - i] > 0)
                     map[25 - i]--;
                 time++;
                 i++;
             }
             Arrays.sort(map);
         }
         return time;
     }
     ```

     

   * 优先级队列

     ```java
     public int leastInterval(char[] tasks, int n) {
         int[] map = new int[26];
         for (char c: tasks)
             map[c - 'A']++;
         PriorityQueue<Integer> queue = 
             new PriorityQueue<>(26, Collections.reverseOrder());
         for (int f: map) {
             if (f > 0)
                 queue.add(f);
         }
         int time = 0;
         while (!queue.isEmpty()) {
             int i = 0;
             List < Integer > temp = new ArrayList < > ();
             while (i <= n) {
                 if (!queue.isEmpty()) {
                     if (queue.peek() > 1)
                         temp.add(queue.poll() - 1);
                     else
                         queue.poll();
                 }
                 time++;
                 if (queue.isEmpty() && temp.size() == 0)
                     break;
                 i++;
             }
             for (int l: temp)
                 queue.add(l);
         }
         return time;
     }
     ```

     

2. 设计，时间O(M)，空间O(1)

   如图：每行为1轮，为了满足相同任务间隔至少为N的条件，空闲时间内任务的分配必须在同一列。

   1. 先看**空闲时间被填满的状况**，只需要在每一轮后面加任务（加任务方式在第2点提及），只要相同的任务不在同一行自然可以满足限制条件，**任务时间 = 任务数M**。

   2. **任务个数大于空闲时间，空闲时间一定能填满吗？ 能。**

      1. **存在某些任务个数比最大任务小至少2。** **没填满的列用下一个任务个数最多的任务X填充，若此列填满后任务X还有剩余，将剩余的任务X从头开始填充下一列。**

         **举个例子：**下图 Figure 2 中的 A,B,C 任务数量不变，D变成 2D，再加上2E和2F。看起来似乎D, E, F各占一列，F似乎需要在每一轮后加一列，造成空闲时间不能被填满。

         **换一种填空的方式。**2D填充一列，用E填充D所在列，剩余的E放在下一行的起头。将E所在的前一列和后一列称为列1和列2。因为E的个数比最大任务小至少2，所以**列1的第一个E和列2的最后一个E相差至少1行，因此这种填空的方式满足限制条件**。

      2. **存在某些任务个数比最大任务小1，正好能填满1列**。

   3. 最后看**空闲时间没填满的情况，任务时间 = 任务数M + 空闲时间**。

    **结论：**
   $$
   idleSlots > 0 ? idleSlots + tasks.length : tasks.length
   $$
   ![Tasks](https://pic.leetcode-cn.com/Figures/621_Task_Scheduler_new.PNG) 

   ```java
   public class Solution {
       public int leastInterval(char[] tasks, int n) {
           int[] map = new int[26];
           for (char c: tasks)
               map[c - 'A']++;
           Arrays.sort(map);
           int max_val = map[25] - 1, idle_slots = max_val * n;
           for (int i = 24; i >= 0 && map[i] > 0; i--) {
               idle_slots -= Math.min(map[i], max_val);
           }
           return idle_slots > 0 ? idle_slots + tasks.length : tasks.length;
       }
   }
   ```

   