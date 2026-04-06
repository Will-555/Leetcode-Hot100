# 难度：中等

给你一个整数数组 `nums` ，请你找出数组中乘积最大的非空连续子数组（该子数组中至少包含一个数字），并返回该子数组所对应的乘积。

测试用例的答案是一个 **32位** 整数。

**示例 1：**
输入：`nums = [2,3,-2,4]`
输出：`6`
解释：子数组 `[2,3]` 有最大乘积 6。

**示例 2：**
输入：`nums = [-2,0,-1]`
输出：`0`
解释：乘积最大子数组是 `[-2,0,-1]` 中的任意一个，结果为 0。

**提示：**
- `1 <= nums.length <= 2 * 10^4`
- `-10 <= nums[i] <= 10`
- `nums` 的任何前缀或后缀的乘积都保证是一个 **32位** 整数。

### 题目分析

本题是 **最长子数组和** 问题（Kadane算法）的变种。不同点在于，负数乘以负数会变成正数。

#### 1. 状态定义
- `maxF[i]`：以第 `i` 个元素结尾的乘积最大值。
- `minF[i]`：以第 `i` 个元素结尾的乘积最小值（主要是为了处理负负得正的情况）。

#### 2. 状态转移
- 如果当前 `nums[i]` 为正数：
  - `maxF[i] = max(nums[i], maxF[i-1] * nums[i])`
  - `minF[i] = min(nums[i], minF[i-1] * nums[i])`
- 如果当前 `nums[i]` 为负数：
  - `maxF[i] = max(nums[i], minF[i-1] * nums[i])`
  - `minF[i] = min(nums[i], maxF[i-1] * nums[i])`
- 如果 `nums[i]` 为 0，则当前值重置为 0。

由于 `dp[i]` 仅依赖于 `dp[i-1]`，可以使用变量记录实现 **$O(1)$ 空间优化**。

#### 3. 复杂度
- **时间复杂度**：$O(n)$，一次遍历。
- **空间复杂度**：$O(1)$，通过滚动变量记录。

### 代码实现

```Java
class Solution {
    public int maxProduct(int[] nums) {
        int maxF = nums[0], minF = nums[0], ans = nums[0];
        int length = nums.length;
        for (int i = 1; i < length; i++) {
            int mx = maxF, mn = minF;
            maxF = Math.max(mx * nums[i], Math.max(nums[i], mn * nums[i]));
            minF = Math.min(mn * nums[i], Math.min(nums[i], mx * nums[i]));
            ans = Math.max(maxF, ans);
        }
        return ans;
    }
}
```
