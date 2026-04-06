# 难度：困难

给你两个单词 `word1` 和 `word2`， 请返回将 `word1`转换成 `word2` 所使用的最少操作数 。

你可以对一个单词进行如下三种操作：
- 插入一个字符
- 删除一个字符
- 替换一个字符

**示例 1：**
输入：`word1 = "horse", word2 = "ros"`
输出：`3`
解释：
`horse -> rorse` (将 'h' 替换为 'r')
`rorse -> rose` (删除 'r')
`rose -> ros` (删除 'e')

**示例 2：**
输入：`word1 = "intention", word2 = "execution"`
输出：`5`
解释：
`intention -> inention` (删除 't')
`inention -> enention` (将 'i' 替换为 'e')
`enention -> exention` (将 'n' 替换为 'x')
`exention -> exection` (将 'n' 替换为 'c')
`exection -> execution` (插入 'u')

**提示：**
- `0 <= word1.length, word2.length <= 500`
- `word1` 和 `word2` 由小写英文字母组成

```Java
class Solution {
    public int minDistance(String word1, String word2) {
        int n = word1.length(), m = word2.length();
        // 确保 word2 是较短的字符串，以优化空间
        if (n < m) return minDistance(word2, word1);
        
        int[] dp = new int[m + 1];
        // 初始化空字符串的情况
        for (int j = 0; j <= m; j++) dp[j] = j;
        
        for (int i = 1; i <= n; i++) {
            int prev = dp[0]; // 相当于 dp[i-1][j-1]
            dp[0] = i; 
            for (int j = 1; j <= m; j++) {
                int temp = dp[j]; // 相当于 dp[i-1][j]
                if (word1.charAt(i - 1) == word2.charAt(j - 1)) {
                    dp[j] = prev;
                } else {
                    // 三种操作：左 (dp[j-1]), 上 (dp[j]), 左上 (prev)
                    dp[j] = Math.min(Math.min(dp[j - 1], dp[j]), prev) + 1;
                }
                prev = temp;
            }
        }
        return dp[m];
    }
}
```
