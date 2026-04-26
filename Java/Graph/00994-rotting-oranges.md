# 难度：中等

在给定的 `m x n` 网格 `grid` 中，每个单元格可以有以下三个值之一：

- 值 `0` 代表空单元格；
- 值 `1` 代表新鲜橘子；
- 值 `2` 代表腐烂的橘子。

每分钟，腐烂的橘子 **周围 4 个方向上相邻** 的新鲜橘子都会腐烂。

返回直到单元格中没有新鲜橘子为止所必须经过的最小分钟数。如果不可能，返回 `-1` 。

**示例 1：**
输入：`grid = [[2,1,1],[1,1,0],[0,1,1]]`
输出：`4`

**示例 2：**
输入：`grid = [[2,1,1],[0,1,1],[1,0,1]]`
输出：`-1`
解释：左下角的橘子（第 2 行，第 0 列）永远不会腐烂，因为腐烂只会发生在 4 个正向上。

**示例 3：**
输入：`grid = [[0,2]]`
输出：`0`
解释：因为 0 分钟时已经没有新鲜橘子了，所以答案就是 0 。

**提示：**
- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 10`
- `grid[i][j]` 仅为 `0`、`1` 或 `2`

## 解题思路

本题是典型的 **多源广度优先搜索 (Multisource BFS)** 问题。

### 为什么使用 BFS？
*   腐烂是每分钟向四周扩散一圈，这非常符合 BFS **按层遍历** 的特性。
*   我们需要求的是“最小分钟数”，在网格图连通性问题中，BFS 遍历的层数直接对应最短路径或最小时间。

### 算法步骤：
1.  **初始化：** 遍历整个网格，将所有**腐烂的橘子 (2)** 的坐标放入队列，同时统计**新鲜橘子 (1)** 的总数。
2.  **多源扩散：**
    *   每一轮（每一分钟）从队列中取出当前所有腐烂的橘子。
    *   尝试向它们的上下左右四个方向扩散。
    *   如果遇到新鲜橘子，将其变为腐烂，新鲜橘子计数减 1，并将其坐标加入队列。
3.  **结果判断：**
    *   如果新鲜橘子计数变为 0，返回当前经过的分钟数。
    *   如果队列空了但新鲜橘子还有剩余，说明有些橘子永远不会腐烂，返回 -1。

## 复杂度分析

*   **时间复杂度：** $O(M \times N)$，其中 $M$ 和 $N$ 是网格的行数和列数。每个格子最多被访问一次。
*   **空间复杂度：** $O(M \times N)$，在最坏情况下，队列中可能存储所有格子的坐标。

## 代码实现

```Java
class Solution {
    public int orangesRotting(int[][] grid) {
        int M = grid.length;
        int N = grid[0].length;
        Queue<int[]> queue = new LinkedList<>();
        int count = 0; // 新鲜橘子数量
        for (int r = 0; r < M; r++) {
            for (int c = 0; c < N; c++) {
                if (grid[r][c] == 1) {
                    count++;
                } else if (grid[r][c] == 2) {
                    queue.add(new int[]{r, c});
                }
            }
        }
        int round = 0; // 腐烂轮数
        while (count > 0 && !queue.isEmpty()) {
            round++;
            int n = queue.size();
            for (int i = 0; i < n; i++) {
                int[] orange = queue.poll();
                int r = orange[0];
                int c = orange[1];
                if (r-1 >= 0 && grid[r-1][c] == 1) {
                    grid[r-1][c] = 2;
                    count--;
                    queue.add(new int[]{r-1, c});
                }
                if (r+1 < M && grid[r+1][c] == 1) {
                    grid[r+1][c] = 2;
                    count--;
                    queue.add(new int[]{r+1, c});
                }
                if (c-1 >= 0 && grid[r][c-1] == 1) {
                    grid[r][c-1] = 2;
                    count--;
                    queue.add(new int[]{r, c-1});
                }
                if (c+1 < N && grid[r][c+1] == 1) {
                    grid[r][c+1] = 2;
                    count--;
                    queue.add(new int[]{r, c+1});
                }
            }
        }
        if (count > 0) {
            return -1;
        } else {
            return round;
        }
    }
}
```
