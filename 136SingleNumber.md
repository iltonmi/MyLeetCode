#  136. Single Number 

1. 位运算，时间O(N), 空间O(1)

   ```java
       public int singleNumber(int[] nums) {
           int res = 0;
           //异或，相同即为假，出现两次的数字异或后抵消变为0
           //将所有数字异或，剩下的数字就是唯一出现的答案
           for(int i : nums) {
               res ^= i;
           }
           return res;
       }
   ```

   

2. 链表，空间O(N), 时间
   $$
   O(n^2)
   $$

   ```java
   public int singleNumber(int[] nums) {
           List<Integer> list = new LinkedList<>();
           boolean found = false;
           for(int i : nums) {
               Iterator itr = list.iterator();
               while(itr.hasNext()) {
                   Integer num = (Integer)itr.next();
                   if(num == i) {
                       itr.remove();
                       found = true;
                       break;
                   }
               }
               if(!found) {
                   list.add(i);
               } else {
                   found = !found;
               }
           }
           return list.get(0);
       }
   ```

   

