#  119. Pascal's Triangle II 

1. dp（原理请看118题）迭代， 空间O(N ^ 2)，时间O(N ^ 2)

   ```java
   public List<Integer> getRow(int rowIndex) {
       int numRows = rowIndex + 1;
       List<List<Integer>> dp = new ArrayList<>(numRows);
       if(numRows <= 0) {
           return new LinkedList<>();
       }
       //pre记录上一行
       List<Integer> pre = new ArrayList<>();
       pre.add(1);
       dp.add(pre);
       for(int i = 2; i <= numRows; i++) {
           List<Integer> cur = new ArrayList<>(i);
           for(int j = 0; j < i; j++) {
               //小心数组越界
               int temp = (j - 1 >= 0 ? pre.get(j - 1) : 0)
                   + (j < pre.size() ? pre.get(j) : 0);
               cur.add(temp);
           }
           dp.add(cur);
           pre = cur;
       }
       return pre;
   }
   ```

   

2. 迭代， 空间O(N)，时间O(N ^ 2)

   ```java
   public List<Integer> getRow(int rowIndex){
       List<Integer> row = new ArrayList<>(rowIndex + 1){
           {
               add(1);
           }
       };
   
   
       for(int i = 0; i < rowIndex; i++){
           for(int j = i; j > 0; j--){
               row.set(j, row.get(j) + row.get(j-1));
           }
           row.add(1);
       }
       return row;
   
   }
   ```

   

3. 递归， 空间O(N)，时间O(N ^ 2)

   ```java
   public List<Integer> getRow(int rowIndex) {
       if(rowIndex == 0) {
           return Arrays.asList(1);
       } else if(rowIndex == 1) {
           return Arrays.asList(1, 1);
       }
       List<Integer> lastRow = getRow(rowIndex-1);
       Integer[] arr = new Integer[rowIndex + 1];
       arr[0] = 1;
       arr[rowIndex] = 1;
       for(int i = 1; i < rowIndex; i++) {
           arr[i] = lastRow.get(i-1) + lastRow.get(i);
       }
   
       return Arrays.asList(arr);
   }
   ```

   