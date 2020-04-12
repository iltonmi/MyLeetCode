#  6. ZigZag Conversion 



1. 无视ZigZag打印顺序中的字符，在字符串中具体位置，只保持相对顺序。空间O(N)，时间O(N)

   ```java
   public String convert(String s, int numRows) {
   
       if (numRows == 1) {
           return s;
       }
       int len = Math.min(numRows, s.length());
       List<StringBuilder> rows = new ArrayList<>(len);
       for (int i = 0; i < len; i++) {
           rows.add(new StringBuilder());
       }
   
       int curRow = 0;
       boolean goingDown = false;
   
       for (char c : s.toCharArray()) {
           rows.get(curRow).append(c);
           if (curRow == 0 || curRow == numRows - 1) {
               goingDown = !goingDown;
           }
           curRow += (goingDown ? 1 : -1);
       }
   
       StringBuilder res = new StringBuilder();
       for (StringBuilder row : rows) {
           res.append(row);
       }
       return res.toString();
   }
   ```

   

2. 考虑具体位置，空间O(N)，时间O(N)

   ```java
   public String convert(String s, int numRows) {
   
       if (numRows == 1) {
           return s;
       }
   
       StringBuilder res = new StringBuilder();
       int n = s.length();
       //cycleLen是在zigzag打印顺序中，第一行的字符之间相差的字符数
       int cycleLen = 2 * numRows - 2;
   
       for (int i = 0; i < numRows; i++) {
           for (int j = 0; j + i < n; j += cycleLen) {
               //唯一字符列：只有唯一字符的列
               //插入每行中，非唯一字符列中的字符
               res.append(s.charAt(j + i));
               if (i != 0 && i != numRows - 1 && j + cycleLen - i < n) {
                   //插入唯一字符列中的字符，第一行和最后一行的唯一字符列上没有字符
                   res.append(s.charAt(j + cycleLen - i));
               }
           }
       }
       return res.toString();
   }
   ```

   