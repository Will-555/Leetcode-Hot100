# 难度：中等

给你一个整数 `n` ，返回 *和为 `n` 的完全平方数的最少数量* 。

**完全平方数** 是一个整数，其值等于另一个整数的平方；换句话说，其值等于一个整数自乘的积。例如，`1`、`4`、`9` 和 `16` 都是完全平方数，而 `3` 和 `11` 不是。

**示例 1：**
输入：`n = 12`
输出：`3` 
解释：`12 = 4 + 4 + 4`

**示例 2：**
输入：`n = 13`
输出：`2`
解释：`13 = 4 + 9`

**提示：**
- `1 <= n <= 10^4`

### 解法一：动态规划 (DP)

这是最通用的解法，适用于此类“最少数量”问题的求解。

*   **时间复杂度**：$O(n \sqrt{n})$
*   **空间复杂度**：$O(n)$

```Java
class Solution {
    public int numSquares(int n) {
        int[] f = new int[n + 1];
        for (int i = 1; i <= n; i++) {
            int minn = Integer.MAX_VALUE;
            for (int j = 1; j * j <= i; j++) {
                minn = Math.min(minn, f[i - j * j]);
            }
            f[i] = minn + 1;
        }
        return f[n];
    }
}
```

### 解法二：数学最优解 (拉格朗日四平方和定理)

这是该特定问题的最优解法。根据定理，结果只能是 1, 2, 3, 4 其中的一个。

*   **时间复杂度**：$O(\sqrt{n})$
*   **空间复杂度**：$O(1)$

```Java
class Solution {
    public int numSquares(int n) {
        // 判断是否为 1
        if (isSquare(n)) return 1;

        // 判断是否为 4 (n = 4^k * (8m + 7))
        int temp = n;
        while (temp % 4 == 0) temp /= 4;
        if (temp % 8 == 7) return 4;

        // 判断是否为 2 (n = a^2 + b^2)
        for (int i = 1; i * i <= n; i++) {
            if (isSquare(n - i * i)) return 2;
        }

        // 排除以上情况，结果为 3
        return 3;
    }

    private boolean isSquare(int n) {
        int root = (int) Math.sqrt(n);
        return root * root == n;
    }
}
```
