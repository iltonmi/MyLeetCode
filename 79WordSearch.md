# 79. Word Search

这道题没有什么技巧，只能遍历每个元素，去匹配第一个字符。

如果第一个字符匹配，就以这个字符为起点，向4个方向使用回溯法寻找可能匹配的字符。

需要注意的地方是，需要记录正在使用中的字符，并在回溯时取消记录。

时间O(M * N * 4L)，空间O(L)，L为字符串长度。

```java
class Solution {
    private String word;
    
    public boolean exist(char[][] board, String word) {
        this.word = word;
        for(int i = 0; i < board.length; i++) {
            for(int j = 0; j < board[0].length; j++) {
                if(board[i][j] == word.charAt(0) && match(board, 0, i, j)) {
                    return true;
                }
            }
        }
        return false;
    }
    
     private boolean match(char[][] board, int next , int i, int j) {
         if(next == word.length()) {
             return true;
         }
         if(i < 0 || i > board.length - 1 || j < 0 || j > board[i].length - 1) {
             return false;
         }
         if(board[i][j] != word.charAt(next++)) {
             return false;
         }
         //标记已被使用的字符
         board[i][j] ^= 256;
         boolean res = false;
         if(match(board, next, i - 1, j)
             || match(board, next, i + 1, j)
             || match(board, next, i, j - 1)
             || match(board, next, i, j + 1)) {
             res = true;
         }
         //回溯
         board[i][j] ^= 256;
         return res;
     }
}
```



 