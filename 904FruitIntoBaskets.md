# 904. Fruit Into Baskets

1. 散列表，滑动窗口，空间O(1)，时间O(N)

   可以用2个变量，速度更快

   ```java
   public int totalFruit(int[] tree) {
       if(tree == null || tree.length == 0) {
           return 0;
       }
       HashMap<Integer, Integer> map = new HashMap<>();
       int count = 0;
       int res = 0;
       for(int r = 0; r < tree.length; r++) {
           int type = tree[r];
           map.put(type, map.getOrDefault(type, 0) + 1);
           count++;
           if(map.size() <= 2) {
               res = Math.max(res, count);
               continue;
           }
           for(int l = r - count + 1; map.size() > 2; l++) {
               int removedType = tree[l];
               int amount = map.get(removedType);
               if(amount == 1) {
                   map.remove(removedType);
               } else {
                   map.put(removedType, amount - 1);
               }
               count--;
           }
       }
       return res;
   }
   ```

    