# 难度：中等

给你一个整数数组 `coins` ，表示不同面额的硬币；以及一个整数 `amount` ，表示总金额。

计算并返回可以凑成总金额所需的 **最少的硬币个数** 。如果没有任何一种硬币组合能组成总金额，返回 `-1` 。

你可以认为每种硬币的数量是无限的。

**示例 1：**
输入：`coins = [1, 2, 5], amount = 11`
输出：`3` 
解释：`11 = 5 + 5 + 1`

**示例 2：**
输入：`coins = [2], amount = 3`
输出：`-1`

**示例 3：**
输入：`coins = [1], amount = 0`
输出：`0`

**提示：**
- `1 <= coins.length <= 12`
- `1 <= coins[i] <= 2^31 - 1`
- `0 <= amount <= 10^4`

### 题目分析

本题是典型的**完全背包问题**变体，考察动态规划。

#### 1. 状态定义
- `dp[i]`：凑成总金额 `i` 所需的最少硬币个数。

#### 2. 状态转移方程
对于每一个金额 `i`，遍历每一枚硬币 `coin`：
- 若 `i >= coin`，则 `dp[i] = min(dp[i], dp[i - coin] + 1)`。
- 这里 `+1` 代表选择了当前的这枚硬币。

#### 3. 初始化与边界条件
- `dp[0] = 0`：凑成金额 0 需要 0 枚硬币。
- 其他 `dp[i]` 初始化为一个不可能的大值（如 `amount + 1`），表示初始状态下所有均无法凑成。
- 最后判断 `dp[amount]` 是否大于 `amount`，若大于则说明无法凑成，返回 `-1`。

#### 4. 复杂度
- **时间复杂度**：$O(\text{amount} \times n)$，其中 $n$ 为硬币种类数。
- **空间复杂度**：$O(\text{amount})$。

### 代码实现

```Java
class Solution {
    public int coinChange(int[] coins, int amount) {
        int max = amount + 1;
        int[] dp = new int[amount + 1];
        Arrays.fill(dp, max);
        dp[0] = 0;
        for (int i = 1; i <= amount; i++) {
            for (int j = 0; j < coins.length; j++) {
                if (coins[j] <= i) {
                    dp[i] = Math.min(dp[i], dp[i - coins[j]] + 1);
                }
            }
        }
        return dp[amount] > amount ? -1 : dp[amount];
    }
}
```
