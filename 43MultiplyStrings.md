#  43. Multiply Strings 

1. 暴力，空间O(M+N)，时间O(M * N)

   ```java
   public String multiply(String num1, String num2) {
       if("0".equals(num1) || "0".equals(num2)) {
           return "0";
       }
       if(num1.length() < num2.length()) {
           String temp = num1;
           num1 = num2;
           num2 = temp;
       }
       //assert num2更短
       int base = 0;
       StringBuilder res = new StringBuilder();
       for(int i = num2.length() - 1; i > -1; i--) {
           StringBuilder val = mul(num1, num2.charAt(i) - '0');
           res = add(res, val.append(zeros(base++)));
       }
       return res.toString();
   }
   
   private String zeros(int amount) {
       StringBuilder sb = new StringBuilder();
       for(int i = 0; i < amount; i++) {
           sb.append("0");
       }
       return sb.toString();
   }
   
   private StringBuilder add(StringBuilder num1, StringBuilder num2) {
        if(num1.length() < num2.length()) {
           StringBuilder temp = num1;
           num1 = num2;
           num2 = temp;
       }
       //assert num2更短
       int diff = num1.length() - num2.length();
       StringBuilder sb = new StringBuilder();
       int c = 0;
       for(int i = num2.length() - 1; i > - 1; i--) {
           int a = num1.charAt(i + diff) - '0';
           int b = num2.charAt(i) - '0';
           int val = a + b + c;
           c = val / 10;
           sb.append(val % 10);
       }
       if(diff != 0) {
           for(int i = diff - 1; i > -1; i--) {
               int val = (num1.charAt(i) - '0') + c;
               c = val / 10;
               sb.append(val % 10);
           }
       }
       if(c != 0) {
           sb.append(c);
       }
       return sb.reverse();
   }
   
   private StringBuilder mul(String str, int a) {
       StringBuilder res = new StringBuilder();
       int c = 0;
       for(int i = str.length() - 1; i > -1; i--) {
           int val = (str.charAt(i) - '0') * a + c;
           c = val / 10;
           res.append(val % 10);
       }
       if(c != 0) {
           res.append(c);
       }
       return res.reverse();
   }
   ```

   

2. 数学规律，属于特定领域问题，硬核，空间O(M+N)，时间O(M * N)

    https://leetcode.com/problems/multiply-strings/discuss/17605/Easiest-JAVA-Solution-with-Graph-Explanation 

   > Remember how we do multiplication?
   >
   > 
   >
   > Start from right to left, perform multiplication on every pair of digits, and add them together. Let's draw the process! From the following draft, we can immediately conclude:
   >
   > 
   >
   > ```java
   >  `num1[i] * num2[j]` will be placed at indices `[i + j`, `i + j + 1]` 
   > ```
   >
   > ![图解](https://drscdn.500px.org/photo/130178585/m%3D2048/300d71f784f679d5e70fadda8ad7d68f)

   ```java
   public String multiply(String num1, String num2) {
       int m = num1.length();
       int n = num2.length();
       int[] pos = new int[m + n];
   
       for(int i = m - 1; i >= 0; i--) {
           int a = num1.charAt(i) - '0';
           for(int j = n - 1; j >= 0; j--) {
               int mul = a * (num2.charAt(j) - '0'); 
               int p1 = i + j;
               int p2 = i + j + 1;
               int sum = mul + pos[p2];
   
               pos[p1] += sum / 10;
               pos[p2] = (sum) % 10;
           }
       }  
   
       StringBuilder sb = new StringBuilder();
       for(int p : pos) {
           if(!(sb.length() == 0 && p == 0)) {
               sb.append(p);
           }
       }
       return sb.length() == 0 ? "0" : sb.toString();
   }
   ```

   