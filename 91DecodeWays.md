#  91. Decode Ways 

1. 双数组dp, 空间O(N), 时间O(N)

   ```java
   // if i is single, single[i] = single[i-1] + notsingle[i-1]
   // if i is not single, notsingle[i] = if i can make couple with i-1, single[i-1]
   //                                    else single[i]
   public int numDecodings(String s) {
       int[] sin = new int[s.length()];
       int[] notsin = new int[s.length()];
       //base case
       sin[0] = s.charAt(0) != '0' ? 1 : 0;
       notsin[0] = 0;
       for(int i = 1; i < s.length(); i++) {
           int ch = s.charAt(i) - '0';
           sin[i] = ch != 0 ? sin[i-1] + notsin[i-1] : 0;
           //判断2位数，如果用substring配合Integer.valueOf会从1ms变成2ms
           if((s.charAt(i - 1) - '0') * 10 + ch <= 26) {
               //i-1是0，sin[i-1]也是0
               notsin[i] = sin[i-1];
           } else {
               notsin[i] = 0;
           }
       }
       return sin[s.length()-1] + notsin[s.length()-1];
   }
   ```

   

2. 双数组压缩空间，空间O(1)，时间O(N)

   ```java
   public int numDecodings(String s) {
       int sin = s.charAt(0) != '0' ? 1 : 0;
       int notsin = 0;
       for(int i = 1; i < s.length(); i++) {
           int ch = s.charAt(i) - '0';
           int tempSin = sin;
           sin = ch != 0 ? sin + notsin : 0;
           if((s.charAt(i - 1) - '0') * 10 + ch <= 26) {
               //i-1是0，sin[i-1]也是0
               notsin = tempSin;
           } else {
               notsin = 0;
           }
       }
       return sin + notsin;
   }
   ```

3. 单数组dp，空间O(N)，时间O(N)

   ```java
   public int numDecodings(String s) {
       if(s == null || s.length() == 0) {
           return 0;
       }
       //dp[i] 代表长度为i的字符串的解码方式的数量
       int n = s.length();
       int[] dp = new int[n+1];
       dp[0] = 1;
       dp[1] = s.charAt(0) != '0' ? 1 : 0;
       for(int i = 2; i <= n; i++) {
           int first = Integer.valueOf(s.substring(i-1, i));
           int second = Integer.valueOf(s.substring(i-2, i));
           if(first >= 1 && first <= 9) {
              dp[i] += dp[i-1];  
           }
           if(second >= 10 && second <= 26) {
               dp[i] += dp[i-2];
           }
       }
       return dp[n];
   }
   ```

   

4. 单数组压缩空间，空间O(1)，时间O(N)

   ```java
   public int numDecodings(String s) {
       if(s == null || s.length() == 0) {
           return 0;
       }
       int n = s.length();
       int pre = 1;
       int cur = s.charAt(0) != '0' ? 1 : 0;
       for(int i = 2; i <= n; i++) {
           int first = Integer.valueOf(s.substring(i-1, i));
           int second = Integer.valueOf(s.substring(i-2, i));
           int nextPre = cur;
           if(!(first >= 1 && first <= 9)) {
              cur = 0;  
           }
           if(second >= 10 && second <= 26) {
               cur += pre;
           }
           pre = nextPre;
       }
       return cur;
   }
   ```

   