## Leetcode37. Sudoku Solver
这道题和八皇后问题有些相似，都是填写某个范围内的数，再check是否valid，如果不valid就把它复原就好了，很容易想到用recursion的写法。  
主函数那就是过一遍这个matrix，在空格子那里尝试填写1-9，再用一个helper function去check valid，如果valid了，那就保留这个数，并进入下一轮(call recursion function) 直到所有的空格都填上了数字。注意两点：   
1. recursion function 的 return type 是boolean，这样能够剪枝，对于那些已经false的情况，就不必再往下走了；  
2. check valid时，行和列都很容易check，但是3*3的小矩阵需要稍微动下脑子，也不难。

```java
class Solution {
  public void solverSudoku(char[][] board) {
    solver(board);
  } 
  private boolean solver(char[][] board) {
    for(int i = 0; i < board.length; i++) {
      for(int j = 0; j < board[0].length; j++) {
        if(board[i][j] == '.') {
          for(char c = '1'; c <= '9'; c++) {
            if(valid(board, i, j, c)) {
              board[i][j] = c;
              
              if(solver(board)) { 
                return true;
              } else {
                board[i][j] = '.'; //下面有不valid的，这里重置，以便尝试下一个c
              }
            }
          }
          return false; // 不valid直接return false；
        }
      }
    }
    return true;
  }
  private boolean valid(char[][] board, int row, int col, char c) {
    for(int i = 0; i < 9; i++) {
      if(board[row][i] == c) {
        return false;
      } 
      if(board[i][col] == c) {
        return false;
      }
      if(board[row/3 * 3 + i/3][col/3 * 3 + i%3] == c) {
        return false;
      }
    }
    return true;
  }
}
```
