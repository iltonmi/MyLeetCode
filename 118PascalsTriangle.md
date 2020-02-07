#  118. Pascal's Triangle

> 资料来源于*The algorithm design manual*

![image-20200207134807410](C:\Users\weili\AppData\Roaming\Typora\typora-user-images\image-20200207134807410.png)

第一个表格代表计算的顺序，第二个表格是正确的结果。

**知道了计算顺序，那么如何计算呢？**

>  Each remaining cell is assigned the sum of the cell directly above it and the cell immediately above and to the left.

**翻译：**每个未计算的格子的值，是它正上方格子的值和它正上方的各自的左边的格子的值。

**另外：二项式系数 C(n, k) = a[n, k]。例如：C(5, 4) = a[5, 4] = 5**

1. DP， 空间O(N ^ 2)，时间O(N ^ 2)

   ```java
   public List<List<Integer>> generate(int numRows) {
       List<List<Integer>> res = new ArrayList<>(numRows);
       if(numRows <= 0) {
           return res;
       }
       //pre记录上一行
       List<Integer> pre = new ArrayList<>();
       pre.add(1);
       res.add(pre);
       for(int i = 2; i <= numRows; i++) {
           List<Integer> cur = new ArrayList<>(i);
           for(int j = 0; j < i; j++) {
               //小心数组越界
               int temp = (j - 1 >= 0 ? pre.get(j - 1) : 0)
                   + (j < pre.size() ? pre.get(j) : 0);
               cur.add(temp);
           }
           res.add(cur);
           pre = cur;
       }
       return res;
}
   ```
   
   