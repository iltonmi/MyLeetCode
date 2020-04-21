#  93. Restore IP Addresses 

1. 回溯

   ```java
   class Solution {
       private List<String> solutions = new ArrayList<>();
       
       public List<String> restoreIpAddresses(String s) {
           restoreIp(s, 0, new StringBuilder(), 0);
           return solutions;
       }
   
       /**
       * idx是切割点的前一个字符
       */
       private void restoreIp(String ip, int idx, StringBuilder temp, int count) {
           if (count > 4) {
               return;
           }
           if (count == 4 && idx == ip.length()) {
               solutions.add(temp.substring(0, temp.length() - 1));
               return;
           }
   
           for (int i = 1; i < 4; i++) {
               if (idx + i > ip.length()) {
                   break;
               }
               String s = ip.substring(idx, idx + i);
               if ((s.startsWith("0") && s.length() > 1) 
                   || (i == 3 && Integer.parseInt(s) >= 256)) {
                   continue;
               }
               int oldLen = temp.length();
               restoreIp(ip, idx + i, temp.append(s).append("."), count + 1);
               temp.delete(oldLen, temp.length());
           }
       }
   }
   ```

   