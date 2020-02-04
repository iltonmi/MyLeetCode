#  13. Roman to Integer

这道题主要在于对罗马数字的理解。。。。。。。

没有普适性。。。。。。。。

1. 正向分步扫描，第一步确定负数，第二步确定正数

   ```java
   class Solution {
       public int romanToInt(String s) {
           int sum=0;
           //减双倍，后面扫描加一倍，相当于减了一倍
           if(s.indexOf("IV")!=-1){sum-=2;}
           if(s.indexOf("IX")!=-1){sum-=2;}
           if(s.indexOf("XL")!=-1){sum-=20;}
           if(s.indexOf("XC")!=-1){sum-=20;}
           if(s.indexOf("CD")!=-1){sum-=200;}
           if(s.indexOf("CM")!=-1){sum-=200;}
   
           char c[]=s.toCharArray();
   
          for(int count=0;count<=s.length()-1;count++){
              if(c[count]=='M') sum+=1000;
              if(c[count]=='D') sum+=500;
              if(c[count]=='C') sum+=100;
              if(c[count]=='L') sum+=50;
              if(c[count]=='X') sum+=10;
              if(c[count]=='V') sum+=5;
              if(c[count]=='I') sum+=1;
   
          }
   
          return sum;
   
       }
   }
   ```

   

2. 反向遍历，罗马数字的特性，只有大于5，I代表负数；只有大于50，X代表负数；只有大于500，C 代表负数。

   ```java
   public static int romanToInt(String s) {
   	int res = 0;
   	for (int i = s.length() - 1; i >= 0; i--) {
   		char c = s.charAt(i);
   		switch (c) {
   		case 'I':
   			res += (res >= 5 ? -1 : 1);
   			break;
   		case 'V':
   			res += 5;
   			break;
   		case 'X':
   			res += 10 * (res >= 50 ? -1 : 1);
   			break;
   		case 'L':
   			res += 50;
   			break;
   		case 'C':
   			res += 100 * (res >= 500 ? -1 : 1);
   			break;
   		case 'D':
   			res += 500;
   			break;
   		case 'M':
   			res += 1000;
   			break;
   		}
   	}
   	return res;
   }
   ```

   