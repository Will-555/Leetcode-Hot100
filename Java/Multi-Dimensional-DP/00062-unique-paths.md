# 难度：中等

一个机器人位于一个 `m x n` 网格的左上角 （起始点在下图中标记为 “Start” ）。

机器人每次只能向下或者向右移动一步。机器人试图达到网格的右下角（在下图中标记为 “Finish” ）。

问总共有多少条不同的路径？

**示例 1：**
![Unique Paths](https://assets.leetcode.com/uploads/2018/10/22/robot_maze.png)
输入：`m = 3, n = 7`
输出：`28`

**示例 2：**
输入：`m = 3, n = 2`
输出：`3`
解释：
从左上角开始，总共有 3 条路径可以到达右下角。
1. 向右 -> 向下 -> 向下
2. 向下 -> 向下 -> 向右
3. 向下 -> 向右 -> 向下

**提示：**
- `1 <= m, n <= 100`
- 题目数据保证答案小于等于 `2 * 10^9`

```Java
class Solution {
    public int uniquePaths(int m, int n) {
        int[] dp = new int[n];
        // 初始化第一行
        for (int i = 0; i < n; i++) dp[i] = 1;
        
        for (int i = 1; i < m; i++) {
            // 每行的第一列都是 1
            for (int j = 1; j < n; j++) {
                // dp[j] = dp[j] (上方) + dp[j-1] (左侧)
                dp[j] += dp[j - 1];
            }
        }
        return dp[n - 1];
    }
}
```
