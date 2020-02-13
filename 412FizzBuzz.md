#  412. Fizz Buzz

1. 没啥好说的，空间O(1), 时间O(N)。

   ```java
   public List<String> fizzBuzz(int n) {
       List<String> res = new ArrayList<>(n);
       for(int i = 1; i <= n; i++) {
           res.add(fizzBuzzStr(i));
       }
       return res;
   }
   
   private String fizzBuzzStr(int i) {
       if(i % 15 == 0) {
           return "FizzBuzz";
       }
       if(i % 3 == 0) {
           return "Fizz";
       }
       if(i % 5 == 0) {
           return "Buzz";
       }
       return String.valueOf(i);
   }
   ```

   