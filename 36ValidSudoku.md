#  36. Valid Sudoku（有效数独）

 https://leetcode-cn.com/problems/valid-sudoku/solution/you-xiao-de-shu-du-by-leetcode/ 

索然无味的散列表，唯一的新发现是，将行列号映射到子数独。

```java
//官方题解改良版本
public boolean isValidSudoku(char[][] board) {
    // init data
    HashSet<Integer> [] rows = new HashSet[9];
    HashSet<Integer> [] columns = new HashSet[9];
    HashSet<Integer> [] boxes = new HashSet[9];
    for (int i = 0; i < 9; i++) {
        rows[i] = new HashSet<Integer>();
        columns[i] = new HashSet<Integer>();
        boxes[i] = new HashSet<Integer>();
    }

    // validate a board
    for (int i = 0; i < 9; i++) {
        for (int j = 0; j < 9; j++) {
            char num = board[i][j];
            if (num == '.') {
                continue;
            }
            int n = (int) num;
            int box_index = (i / 3 ) * 3 + j / 3;
            // check if this value has been already seen before
            if (rows[i].contains(n) || columns[j].contains(n) 
              || boxes[box_index].contains(n)) {
                return false;
            }
            // keep the current cell value
            rows[i].add(n);
            columns[j].add(n);
            boxes[box_index].add(n);
        }
    }

    return true;
}
```

