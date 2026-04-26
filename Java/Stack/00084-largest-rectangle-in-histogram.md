# 84. 柱状图中最大的矩形 (Largest Rectangle in Histogram)

[LeetCode 题目链接](https://leetcode.cn/problems/largest-rectangle-in-histogram/)

## 题目描述

给定 $n$ 个非负整数，用来表示柱状图中各个柱子的高度。每个柱子彼此相邻，且宽度为 1 。

求在该柱状图中，能够勾勒出来的矩形的最大面积。

## 解题思路

本题的核心在于：**如何确定以某根柱子为高度的矩形的最大宽度？**
以第 $i$ 根柱子（高度为 $h_i$）为高的矩形，其宽度取决于左边第一个比它矮的柱子和右边第一个比它矮的柱子。

使用 **单调栈 (Monotonic Stack)** 可以在 $O(N)$ 时间内找到所有柱子的左右边界。

### 算法步骤：
1.  **哨兵技巧：** 在原高度数组的前后各添加一个高度为 0 的柱子。
    *   前面的 0 可以避免非空判断。
    *   后面的 0 可以强制触发栈中所有剩余柱子的结算。
2.  **维护单调递增栈：** 栈中存储柱子的 **下标**。
3.  **遍历数组：**
    *   如果当前柱子高度小于栈顶柱子高度：
        *   说明栈顶柱子的右边界已找到（即当前索引）。
        *   弹出栈顶柱子作为“高” $h$。
        *   弹出后的新栈顶就是该柱子的左边界。
        *   宽度 $w = current\_index - stack\_top\_index - 1$。
        *   面积 $area = h \times w$。
    *   将当前柱子下标压入栈中。

## 代码实现

```java
import java.util.Stack;

class Solution {
    public int largestRectangleArea(int[] heights) {
        int n = heights.length;
        // 1. 添加哨兵，简化边界处理
        int[] newHeights = new int[n + 2];
        System.arraycopy(heights, 0, newHeights, 1, n);
        newHeights[0] = 0;
        newHeights[n + 1] = 0;

        Stack<Integer> stack = new Stack<>();
        int maxArea = 0;

        for (int i = 0; i < newHeights.length; i++) {
            // 2. 发现当前柱子比栈顶矮时，开始结算
            while (!stack.isEmpty() && newHeights[i] < newHeights[stack.peek()]) {
                int h = newHeights[stack.pop()]; // 确定的高度
                int w = i - stack.peek() - 1;    // 确定的宽度
                maxArea = Math.max(maxArea, h * w);
            }
            // 3. 始终保持栈内元素高度单调递增
            stack.push(i);
        }

        return maxArea;
    }
}
```

## 复杂度分析

*   **时间复杂度：** $O(N)$。每个柱子最多进栈一次、出栈一次。
*   **空间复杂度：** $O(N)$。使用了栈来辅助计算，最坏情况下栈需要存储 $N$ 个柱子的下标。
