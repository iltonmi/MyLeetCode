# 168. Excel Sheet Column Title

1. 递归

   ```java
   public String convertToTitle(int n) {
       return n == 0 ? "" : convertToTitle(--n / 26) + (char)('A' + (n % 26));
   }
   ```

   

2. 迭代

   ```java
   public String convertToTitle(int n) {
       StringBuilder res = new StringBuilder();
   
       while(n-- > 0){
           //n - 1是因为 'A' + 0 = 'A', 而题意中'A' = 1, 所以需要一个offset=1
           res.insert(0, (char)('A' + n % 26));
           n /= 26;
       }
   
       return res.toString();
   }
   ```

   