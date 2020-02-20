#  207. Course Schedule 

利用拓扑排序找出循环依赖。参考本人专门讲拓扑排序的文章。

1. Kahn算法，利用拓扑排序，若入队列的课程数量不等于所需课程数量，则false。空间O(N)，时间O(N)

   ```java
   class Solution {
       public boolean canFinish(int numCourses, int[][] prerequisites) {
           List[] map = new List[numCourses];
           int[] inDegree = new int[numCourses];
           for(int i = 0; i < numCourses; ++i) {
               map[i] = new ArrayList<>();
           }
           for(int[] entry : prerequisites) {
               int pre = entry[1];
               int next = entry[0];
               map[pre].add(next);
               inDegree[next]++;
           }
           Deque<Integer> q = new LinkedList<>();
           int learned = 0;
           for(int i = 0; i < numCourses; ++i) {
               if(inDegree[i] == 0) {
                   q.add(i);
                   ++learned;
               }
           }
           while(!q.isEmpty()) {
               List<Integer> list = map[q.poll()];
               for(Integer i : list) {
                   if(--inDegree[i] == 0) {
                       q.add(i);
                       learned++;
                   }
               }
           }
           return learned == numCourses;
       }
   }
   ```

   

2. DFS算法，利用拓扑排序，若存在节点discovered但未processed，则说明存在循环依赖。空间O(N)，时间O(N)。

   ```java
   class Solution{
       public boolean canFinish(int numCourses, int[][] prerequisites){
        ArrayList<ArrayList<Integer>> graph = new ArrayList<>(numCourses);
           
           for(int i = 0; i< numCourses; ++i){
               graph.add(new ArrayList<>());
           }
           
           for(int i = 0; i< prerequisites.length; ++i){
               graph.get(prerequisites[i][0]).add(prerequisites[i][1]);
           }
           
           int[] visited = new int[numCourses];
           for(int i = 0; i < numCourses; ++i)
               if(!dfs(i, graph, visited)) return false;
           
           return true;
       }
       
       /**
       * 0 for undiscovered
       * 1 for discovered
       * 2 for processed
       **/
       private boolean dfs(int cur, ArrayList<ArrayList<Integer>> graph, int[] visited){
           if(visited[cur] == 1) {
               return false;
           }
           if(visited[cur] == 2) {
               return true;
           }
           
           visited[cur] = 1;
           
           for(int next:graph.get(cur))
               if(!dfs(next, graph, visited)) return false;
           
           visited[cur] = 2;
           return true;
           
       }
   }
   ```
   
   