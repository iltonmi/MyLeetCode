#  12. Integer to Roman 

这属于特殊题型。。。。。。。。。。。

num 的范围是1-3999。

1. 一目了然，解决了4、9，90等数字的表达

   ```java
   public String intToRoman(int num) {
   
       int[] values = {1000,900,500,400,100,90,50,40,10,9,5,4,1};
       String[] strs = {"M","CM","D","CD","C","XC","L","XL","X","IX","V","IV","I"};
   
       StringBuilder sb = new StringBuilder();
   
       for(int i=0;i<values.length;i++) {
           while(num >= values[i]) {
               num -= values[i];
               sb.append(strs[i]);
           }
       }
       return sb.toString();
   }
   ```

   

2. 