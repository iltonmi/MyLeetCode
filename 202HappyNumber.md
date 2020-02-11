#  202. Happy Number

这道题是个数学题，基于一个观察。

1. 观察1：一个3位数999经过处理之后的值是243，显然每个位置的值都小于9，所以这个值无论如何都不可能大于999。

   结论1：序列中的值存在上限。

2. 分析1：根据结论1，序列的值存在上限，又因为序列是无限的，序列值必定存在重复。

   推论1：序列的值存在重复。

3. 分析2：根据数学规律，所有值的下一个值都是固定的，所以序列值存在循环。

   推论2：序列的值存在循环。

到这里，这已经变成一道循环检测的题目了，和链表环检测同理。

1. Floyd，快慢指针

   ```java
   public boolean isHappy(int n) {
       int slow = n;
       int fast = next(n);
       while(fast != 1) {
           if(slow == fast) {
               return false;
           }
           slow = next(slow);
           fast = next(next(fast));
       }
       return true;
   }
   
   private int next(int n) {
       int res = 0;
       int nextDigit;
       while (n > 0) {
           nextDigit = n % 10;
           res += nextDigit * nextDigit;
           n /= 10;
       }
       return res;
   }
   ```

   

2. 散列表

   ```java
   public boolean isHappy(int n) {
       Set<Integer> set = new HashSet<Integer>();
       while (set.add(n)) {
           int nextVal = 0;
           int nextDigit;
           //计算下一个值
           while (n > 0) {
               nextDigit = n % 10;
               nextVal += nextDigit * nextDigit;
               n /= 10;
           }
           if (nextVal == 1) {
               return true;
           } else {
               n = nextVal;   
           }
       }
       return false;
   }
   ```

   