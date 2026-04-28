# 难度：中等

给你一个字符串 `s`，请你将 `s` 分割成一些子串，使每个子串都是 **回文串** 。返回 `s` 所有可能的分割方案。

**示例 1：**
输入：`s = "aab"`
输出：`[["a","a","b"],["aa","b"]]`

**示例 2：**
输入：`s = "a"`
输出：`[["a"]]`

**提示：**
- `1 <= s.length <= 16`
- `s` 仅由小写英文字母组成

```Java
class Solution {
    List<List<String>> res = new ArrayList<>();
    List<String> path = new ArrayList<>();
    boolean[][] dp;

    public List<List<String>> partition(String s) {
        int n = s.length();
        dp = new boolean[n][n];
        // 预处理：计算所有子串是否为回文串
        for (int i = 0; i < n; i++) {
            Arrays.fill(dp[i], true);
        }
        for (int i = n - 1; i >= 0; i--) {
            for (int j = i + 1; j < n; j++) {
                dp[i][j] = (s.charAt(i) == s.charAt(j)) && dp[i + 1][j - 1];
            }
        }

        backtrack(s, 0);
        return res;
    }

    private void backtrack(String s, int start) {
        if (start == s.length()) {
            res.add(new ArrayList<>(path));
            return;
        }

        for (int i = start; i < s.length(); i++) {
            if (dp[start][i]) {
                path.add(s.substring(start, i + 1));
                backtrack(s, i + 1);
                path.remove(path.size() - 1);
            }
        }
    }
}

## 题解说明

### 核心思路

本题要求将字符串分割成多个回文子串。由于需要列出所有可能的分割方案，且每一步分割都依赖于前一步的结果，这显然是一个**回溯算法**的应用场景。

### 1. 回溯法 (Backtracking)
回溯的过程本质上是在扫描字符串。对于当前位置 `start`，我们枚举所有可能的分割点 `i`：
*   **做出选择**：如果子串 `s[start...i]` 是回文串，我们就将其加入当前路径 `path`。
*   **递归调用**：继续从 `i + 1` 的位置开始分割剩余的字符串。
*   **撤销选择 (回溯)**：将刚才加入 `path` 的子串移除，尝试下一个分割点。

### 2. 动态规划预处理 (DP Optimization)
在回溯过程中，我们需要频繁判断子串 `s[start...i]` 是否为回文。如果每次都使用双指针检查，会产生大量重复计算。
我们使用**动态规划 (DP)** 预处理出字符串中所有子串是否为回文的情况：
*   状态定义：`dp[i][j]` 表示子串 `s[i...j]` 是否为回文串。
*   状态转移：
    *   如果 `s[i] == s[j]` 且内部子串 `s[i+1...j-1]` 也是回文，则 `dp[i][j]` 为 `true`。
    *   在代码实现中，我们采用倒序遍历 `i` 的方式来确保计算 `dp[i][j]` 时，`dp[i+1][j-1]` 已经计算完毕。

### 复杂度分析
*   **时间复杂度**：$O(n \cdot 2^n)$。在最坏情况下（如字符串全由相同字符组成），分割方案数量级为 $2^n$，每个方案拷贝到结果集需要 $O(n)$。
*   **空间复杂度**：$O(n^2)$。主要用于存储 $n \times n$ 的 DP 预处理数组，递归深度最多为 $n$。
