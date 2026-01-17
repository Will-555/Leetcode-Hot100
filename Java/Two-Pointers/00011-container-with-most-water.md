# 难度：中等

给定一个长度为 `n` 的整数数组 `height` 。有 `n` 条垂线，第 `i` 条线的两个端点是 `(i, 0)` 和 `(i, height[i])` 。

找出其中的两条线，使得它们与 `x` 轴共同构成的容器可以容纳最多的水。

返回容器可以储存的最大水量。

**说明：**你不能倾斜容器。

**示例 1：**
输入：`[1,8,6,2,5,4,8,3,7]`
输出：`49` 
解释：图中垂直线代表输入数组 `[1,8,6,2,5,4,8,3,7]`。在此场景中，垂直线 `height[i]=8` 和 `height[j]=7` 能够容纳的水量最大。

**示例 2：**
输入：`height = [1,1]`
输出：`1`

**提示：**
- `n == height.length`
- `2 <= n <= 10^5`
- `0 <= height[i] <= 10^4`

```Java
class Solution {
    public int maxArea(int[] height) {
        int left = 0, right = height.length - 1, ans = 0;
        while (left < right) {
            int area = Math.min(height[left], height[right]) * (right - left);
            ans = Math.max(ans, area);
            if (height[left] < height[right]) left++;
            else right--;
        }
        return ans;
    }
}
```
