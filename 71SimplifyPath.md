#  71. Simplify Path 

1. 常规思路，2套API（Stack和Deque），空间O(N)，时间O(N)

   Stack的push，底层是Vector的add，插入元素到容器的末尾

   Deque的接口，规定了push的实现，必须是插入元素到容器的头部，包括ArrayDeque和LinkedList都一样

   结论是：for-each迭代容器的顺序，Stack是栈底到栈顶，Deque是栈顶到栈底

   ```java
   public String simplifyPath(String path) {
       if (path == null || path.isEmpty()) {
           return "";
       }
       Stack<String> stack = new Stack<>();
       stack.add("/");
       for (int i = 1; i < path.length();) {
           if (path.charAt(i) == '/') {
               i++;
               continue;
           }
   
           int j = i + 1;
           for (; j < path.length(); j++) {
               if (path.charAt(j) == '/') {
                   break;
               }
           }
   
           String dir = path.substring(i, j);
           //若返回上级目录，且当前目录不是根目录
           if ("..".equals(dir) && stack.size() != 1) {
               stack.pop();
           }
           //若不返回上级目录，且不是当前目录（即"."）
           if (!"..".equals(dir) && !".".equals(dir)) {
               stack.add(dir + "/");
           }
   
           i = j + 1;
       }
   
       StringBuilder sb = new StringBuilder();
       //2个接口的不同在于迭代顺序不同
       for (String tmp : stack) {
           sb.append(tmp);
       }
       if (sb.length() > 1) {
           sb.deleteCharAt(sb.length() - 1);
       }
   
       return sb.toString();
   }
   public String simplifyPath(String path) {
       if (path == null || path.isEmpty()) {
           return "";
       }
       Deque<String> stack = new ArrayDeque<>(path.length());
       stack.push("/");
       for (int i = 1; i < path.length();) {
           if (path.charAt(i) == '/') {
               i++;
               continue;
           }
   
           int j = i + 1;
           for (; j < path.length(); j++) {
               if (path.charAt(j) == '/') {
                   break;
               }
           }
   
           String dir = path.substring(i, j);
           //若返回上级目录，且当前目录不是根目录
           if ("..".equals(dir) && stack.size() != 1) {
               stack.pop();
           }
           //若不返回上级目录，且不是当前目录（即"."）
           if (!"..".equals(dir) && !".".equals(dir)) {
               stack.push(dir + "/");
           }
   
           i = j + 1;
       }
   
       StringBuilder sb = new StringBuilder();
       while(!stack.isEmpty()) {
           //2个接口的不同在于迭代顺序不同
           sb.append(stack.removeLast());
       }
       if (sb.length() > 1) {
           sb.deleteCharAt(sb.length() - 1);
       }
   
       return sb.toString();
   }
   ```

   

2. 投机取巧

   ```java
   public String simplifyPath(String path) {
       Deque<String> stack = new ArrayDeque<>();
       Set<String> skip = new HashSet<>(Arrays.asList("..",".",""));
       for (String dir : path.split("/")) {
           if (dir.equals("..") && !stack.isEmpty()) {
               //返回上级目录，且当前不是根目录
               stack.pop();
           } else if (!skip.contains(dir)) {
               //不包含2个"/"之间的空白、返回上一级目录、当前目录 
               stack.push(dir);
           }
       }
       StringBuilder res = new StringBuilder();
       while(!stack.isEmpty()) {
           res.append("/" + stack.removeLast());
       }
       return res.length() == 0 ? "/" : res.toString();
   }
   ```

   