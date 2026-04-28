# 难度：困难

**n 皇后问题** 研究的是如何将 `n` 个皇后放置在 `n × n` 的棋盘上，并且使皇后彼此之间不能相互攻击。

给你一个整数 `n` ，返回所有不同的 **n 皇后问题** 的解决方案。

每一种解法包含一个不同的 **n 皇后问题** 的棋子放置方案，该方案中 `'Q'` 和 `'.'` 分别代表了皇后和空位。

**示例 1：**
输入：`n = 4`
输出：`[[".Q..","...Q","Q...","..Q."],["..Q.","Q...","...Q",".Q.."]]`
解释：如上图所示，4 皇后问题存在两个不同的解法。

**示例 2：**
输入：`n = 1`
输出：`[["Q"]]`

**提示：**
- `1 <= n <= 9`

```Java
class Solution {
    List<List<String>> res = new ArrayList<>();
    // 用于标记某列、某条对角线是否已经放置了皇后
    boolean[] columns;
    boolean[] diagonals1;
    boolean[] diagonals2;

    public List<List<String>> solveNQueens(int n) {
        columns = new boolean[n];
        // 对角线数量为 2n-1
        diagonals1 = new boolean[2 * n - 1]; // 主对角线标记: row - col 为常数
        diagonals2 = new boolean[2 * n - 1]; // 副对角线标记: row + col 为常数
        
        // 初始化棋盘，初始全为 '.'
        char[][] board = new char[n][n];
        for (int i = 0; i < n; i++) {
            Arrays.fill(board[i], '.');
        }
        
        // 从第 0 行开始回溯
        backtrack(board, 0, n);
        return res;
    }

    private void backtrack(char[][] board, int row, int n) {
        // 终止条件：所有的行都过了一遍，说明找到了一个合法解
        if (row == n) {
            res.add(construct(board));
            return;
        }

        // 枚举当前行的每一列，尝试放置皇后
        for (int col = 0; col < n; col++) {
            // 计算当前位置对应的两条对角线的索引
            // row - col 的范围是 [-(n-1), n-1]，加上 n-1 映射到 [0, 2n-2]
            int d1 = row - col + n - 1; 
            // row + col 的范围是 [0, 2n-2]
            int d2 = row + col;

            // 检查当前列、两条对角线是否冲突
            if (!columns[col] && !diagonals1[d1] && !diagonals2[d2]) {
                // 1. 做选择：在当前位置放置皇后
                board[row][col] = 'Q';
                columns[col] = true;
                diagonals1[d1] = true;
                diagonals2[d2] = true;

                // 2. 递归：进入下一行进行决策
                backtrack(board, row + 1, n);

                // 3. 撤销选择（回溯）：恢复现场
                board[row][col] = '.';
                columns[col] = false;
                diagonals1[d1] = false;
                diagonals2[d2] = false;
            }
        }
    }

    // 将字符数组形式的棋盘转化为题目要求的 List<String> 格式
    private List<String> construct(char[][] board) {
        List<String> path = new ArrayList<>();
        for (int i = 0; i < board.length; i++) {
            path.add(new String(board[i]));
        }
        return path;
    }
}
```
