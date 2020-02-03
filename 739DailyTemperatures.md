#  739. Daily Temperatures

最近最大或者最近最小都是单调栈结构

1. 使用单调栈结构找到右边最近的更大值，空间O(N)，时间O(N)

   ```java
   class Solution {
       public int[] dailyTemperatures(int[] T) {
           int[] res = new int[T.length];
           Deque<Integer> stk = new ArrayDeque<>(T.length);
           for(int i = 0; i < T.length; i++) {
               while(!stk.isEmpty() && T[i] > T[stk.peek()]) {
                   int min = stk.pop();
                   res[min] = i - min;
               }
               stk.push(i);
           }
           while(!stk.isEmpty()) {
               res[stk.pop()] = 0;
           }
           return res;
       }
   }
   ```

   ```java
   class Solution {
       public int[] dailyTemperatures(int[] T) {
           int length =  T.length;
           int[] res = new int[length];
           int[] stk = new int[length];
           int head=-1;
   
           for(int i=0; i< length;i++){
               
               while( head>=0 && T[i] > T[stk[head]]){
                   int min = stk[head--];
                   res[min] = i-min;
               }
   
               stk[++head]=i;
           }
   
           return res;
       }
   }
   ```

   

2. next数组

    https://leetcode.com/articles/daily-temperatures/ 

   从后往前遍历，使用 next 数组记录温度出现的最新位置。
   
   ```java
   class Solution {
       public int[] dailyTemperatures(int[] T) {
           int[] res = new int[T.length];
           int[] next = new int[101];
           Arrays.fill(next, Integer.MAX_VALUE);
           //从后往前遍历
           for (int i = T.length - 1; i >= 0; --i) {
               int warmerIndex = Integer.MAX_VALUE;
               for (int t = T[i] + 1; t <= 100; ++t) {
                   //找到最近的位置
                   if (next[t] < warmerIndex) {
                       warmerIndex = next[t];
                   }              
               }
               if (warmerIndex < Integer.MAX_VALUE) {
                   res[i] = warmerIndex - i;
               }
            //更新当前温度的最近位置
               next[T[i]] = i;
           }
           return res;
       }
   }
   ```
   
   