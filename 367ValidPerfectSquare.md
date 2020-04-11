#  367. Valid Perfect Square



1. 二分查找，空间O(1)，时间O(N)

   ```java
   public boolean isPerfectSquare(int num) {
       int low = 1, high = num;
       while (low <= high) {
           long mid = (low + high) >>> 1;
           if (mid * mid == num) {
               return true;
           } else if (mid * mid < num) {
               low = (int) mid + 1;
           } else {
               high = (int) mid - 1;
           }
       }
       return false;
   }
   ```

   

2. 数学规律

   ```java
   public boolean isPerfectSquare(int num) {
        int i = 1;
        while (num > 0) {
            num -= i;
            i += 2;
        }
        return num == 0;
    }
   ```

   