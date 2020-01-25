#  (mark)240. Search a 2D Matrix II 

按照某种规律有序的数组，当然要试一试二分查找啦。

1. 暴力，空间O(1)，时间O(MN)

2. 比较target和中间列，划分矩阵，分而治之。只可能出现在左下角和右上角的矩阵中。

   空间O(logN)，时间:
   $$
   T(x)=2⋅T(\frac{x}{4})+\frac{x}{2}=O(x)
   $$
   

   * 矩阵的最小值：左上角。矩阵的最大值：右下角。

   * 递归的基本条件：数组不包含元素，target小于最小值，或target大于最大值。

   将target与矩阵中间列元素比较，直到matrix [row] [mid] > target。

   * 递归条件：

   此时，矩阵以(row, mid)所在的行和列分为4份。

   由于(row, mid)上的元素都小于target，所以target > (row - 1, mid)为右下角的矩阵，即紫色部分。

   由于(row, mid) > target，所以target < (row, mid)为左上角的矩阵，即蓝色部分。

   因此，target可能出现的范围被缩小到2个矩阵，即粉色和绿色的部分。

   <table>
       <tr>
           <td bgcolor=#7B68EE>/</td> 
           <td bgcolor=#7B68EE>/</td> 
           <td bgcolor=#7B68EE>/</td> 
           <td bgcolor=#7B68EE>/</td> 
           <td bgcolor=#FF69B4>/</td>
           <td bgcolor=#FF69B4>/</td>
      </tr>
       <tr>
           <td bgcolor=#7B68EE>/</td> 
           <td bgcolor=#7B68EE>/</td> 
           <td bgcolor=#7B68EE>/</td> 
           <td bgcolor=#7B68EE>/</td> 
           <td bgcolor=#FF69B4>/</td>
           <td bgcolor=#FF69B4>/</td>   
       </tr>
       <tr>
           <td bgcolor=#7B68EE>/</td> 
           <td bgcolor=#7B68EE>/</td> 
           <td bgcolor=#7B68EE>/</td> 
           <td bgcolor=#7B68EE>/</td> 
           <td bgcolor=#FF69B4>/</td>
           <td bgcolor=#FF69B4>/</td>   
       </tr>
       <tr>
           <td bgcolor=#ADFF2F>/</td>
           <td bgcolor=#ADFF2F>/</td>
           <td bgcolor=#ADFF2F>/</td>
           <td bgcolor=#00F5FF>(row, mid)</td>
           <td bgcolor=#00F5FF>/</td>
           <td bgcolor=#00F5FF>/</td>
       </tr>
       <tr>
           <td bgcolor=#ADFF2F>/</td>
           <td bgcolor=#ADFF2F>/</td>
           <td bgcolor=#ADFF2F>/</td>
           <td bgcolor=#00F5FF>/</td>
           <td bgcolor=#00F5FF>/</td>
           <td bgcolor=#00F5FF>/</td>
       </tr>
       <tr>
           <td bgcolor=#ADFF2F>/</td>
           <td bgcolor=#ADFF2F>/</td>
           <td bgcolor=#ADFF2F>/</td>
           <td bgcolor=#00F5FF>/</td>
           <td bgcolor=#00F5FF>/</td>
           <td bgcolor=#00F5FF>/</td>
       </tr>
   </table>

   ```java
   class Solution {
       int[][] matr;
       int target;
       
       public boolean searchMatrix(int[][] matrix, int target) {
           if(matrix == null || matrix.length == 0 || matrix[0].length == 0) {
               return false;
           }
           this.matr = matrix;
           this.target = target;
           return find(0, 0, matrix.length - 1, matrix[0].length - 1);
       }
       
       private boolean find(int up, int left, int down, int right) {
           if(left > right || up > down) {
               return false;
           }
           if(target < matr[up][left] || target > matr[down][right]) {
               return false;
           }
           
           int mid = (left + right) / 2;
           int row = up;
           while(row <= down && matr[row][mid] <= target) {
               if(matr[row][mid] == target) {
                   return true;
               }
               row++;
           }
           return find(row, left, down, mid - 1) || find(up, mid + 1, row - 1, right);
       }
   }
   ```

   

3. 递归二分查找，空间O(MN)，时间：我觉得这个方法不是真正的二分查找
   $$
   O(log(N!))
   $$

   ```java
   
   ```

   

4. 剪枝，空间O(1)，时间O(M+N)。

   检查4个角落，看看不一致得时候，按照或大或小2种情况观察剩余的解的集合。

   观察发现，从右上角或者左下角出发能够起作用。

   ```java
   class Solution {
       public boolean searchMatrix(int[][] matrix, int target) {
           if(matrix == null || matrix.length == 0 || matrix[0].length == 0) {
               return false;
           }
           int row = 0;
           int col = matrix[0].length - 1;
           while(row < matrix.length && col >= 0) {
               int cur = matrix[row][col];
               if(target == cur) {
                   return true;
               } else if(target < cur) {
                   col--;
               } else {
                   row++;
               }
           }
           return false;
       }
   }
   ```

   