# 210. Course Schedule II

改造Course Schedule即可。

1. kahn算法，空间O(N)，时间O(N)

   ```java
   public int[] findOrder(int numCourses, int[][] prerequisites) {
       List[] map = new List[numCourses];
       int[] inDegree = new int[numCourses];
       int[] res = new int[numCourses];
       for(int i = 0; i < numCourses; ++i) {
           map[i] = new LinkedList<>();
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
               res[learned] = i;
               ++learned;
           }
       }
       while(!q.isEmpty()) {
           List<Integer> list = map[q.poll()];
           for(Integer i : list) {
               if(--inDegree[i] == 0) {
                   res[learned] = i;
                   q.add(i);
                   learned++;
               }
           }
       }
       return learned == numCourses ? res : new int[0];
   }
   ```

   

2. dfs，空间O(N)，时间O(N)

   ```java
   private int[] res;
   private int count = 0;
   public int[] findOrder(int numCourses, int[][] prerequisites){
       ArrayList<ArrayList<Integer>> graph = new ArrayList<>(numCourses);
       for(int i = 0; i< numCourses; ++i){
           graph.add(new ArrayList<>());
       }
   
       for(int i = 0; i< prerequisites.length; ++i){
           graph.get(prerequisites[i][0]).add(prerequisites[i][1]);
       }
   
       int[] visited = new int[numCourses];
       this.res = new int[numCourses];
       for(int i = 0; i < numCourses; ++i)
           if(!dfs(i, graph, visited)) return new int[0];
   
       return res;
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
       this.res[count++] = cur;
       return true;
   
   }
   ```

   