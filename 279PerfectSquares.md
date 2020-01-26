#  （mark）279. Perfect Squares

1. 三平方和定理，四平方和定理，空间O(1)，时间O(sqrt(N))

   ```java
   class Solution {
       // Based on Lagrange's Four Square theorem, there 
       // are only 4 possible results: 1, 2, 3, 4.
       public int numSquares(int n) {
           
           // If n is a perfect square, return 1.
           if(isPerfectSquare(n)) {
               return 1;
           }
           
           // The result is 4 if and only if n can be written
           // in the form of 4^k*(8*m + 7). 
           // Please refer to Legendre's three-square theorem.
           int temp = n;
           //temp % 4 == 0
           while((temp & 3) == 0) {
               temp >>= 2;
           }
           //temp % 8 == 7
           if((temp & 7) == 7) {
               return 4;
           }
           
           // Check whether 2 is the result.
           int sqrtN = (int) Math.sqrt(n);
           for(int i = 1; i <= sqrtN; i++) {
               if(isPerfectSquare(n - i * i)) {
                   return 2;
               }
           }
           return 3;
       }
       
       private boolean isPerfectSquare(int n) {
           return (int) Math.pow(Math.floor(Math.sqrt(n)), 2) == n;
       }
   }
   ```

   

2. BFS，空间O(N)，时间O(N)

   ```java
   class Solution {
       public int numSquares(int n) {
           int[] least = new int[n];
           Deque<Integer> q = new ArrayDeque<>();
           List<Integer> perfectSquares = new ArrayList<>();
           for(int i = 1; i * i <= n; i++) {
               q.add(i * i);
               perfectSquares.add(i * i);
               least[i * i - 1] = 1;
           }
           if(perfectSquares.get(q.size() - 1) == n) {
               return 1;
           }
           int round = 1;
           while(!q.isEmpty()) {
               round++;
               int qSize = q.size();
               for(int i = 0; i < qSize; i++) {
                   Integer cur = q.pollFirst();
                   for(Integer potential : perfectSquares) {
                       if(potential + cur == n) {
                           return round;
                       } else if(potential + cur < n 
                                 && least[potential + cur - 1] == 0) {
                           least[potential + cur - 1] = round;
                           q.addLast(potential + cur);
                       } else if(potential + cur > n) {
                           break;
                       }
                   }
               }
           }
           return 0;
       }
   }
   ```

   

3. 动态规划，空间O(N)，时间O(N^sqrt(N))

   ```java
   class Solution {
       public int numSquares(int n) {
           int[] least = new int[n + 1];
           Arrays.fill(least, Integer.MAX_VALUE);
           least[0] = 0;
           for (int i = 1; i <= n; i++) {
               // For each i, it must be the sum of some number (i - j*j) and 
               // a perfect square number (j*j).
               for (int j = 1; j * j <= i; j++) {
                   least[i] = Math.min(least[i], least[i - j * j] + 1);
               }
           }
           return least[n];
       }
   }
   ```

   