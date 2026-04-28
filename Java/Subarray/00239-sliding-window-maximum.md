# 难度：困难

给你一个整数数组 `nums`，有一个大小为 `k` 的滑动窗口从数组的最左侧移动到数组的最右侧。你只可以看到在滑动窗口内的 `k` 个数字。滑动窗口每次只向右移动一位。

返回 *滑动窗口中的最大值* 。

**示例 1：**
输入：`nums = [1,3,-1,-3,5,3,6,7], k = 3`
输出：`[3,3,5,5,6,7]`
解释：
```
滑动窗口的位置                最大值
---------------               -----
[1  3  -1] -3  5  3  6  7       3
 1 [3  -1  -3] 5  3  6  7       3
 1  3 [-1  -3  5] 3  6  7       5
 1  3  -1 [-3  5  3] 6  7       5
 1  3  -1  -3 [5  3  6] 7       6
 1  3  -1  -3  5 [3  6  7]      7
```

**示例 2：**
输入：`nums = [1], k = 1`
输出：`[1]`

**提示：**
- `1 <= nums.length <= 10^5`
- `-10^4 <= nums[i] <= 10^4`
- `1 <= k <= nums.length`

```Java
class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        int n = nums.length;
        // 双端队列，用来存储数组元素的下标，维持队列内元素从大到小排列（单调队列）
        Deque<Integer> deque = new LinkedList<Integer>();
        
        // 1. 初始化：处理第一个窗口
        for (int i = 0; i < k; ++i) {
            // 如果新进入的元素大于等于队尾元素，队尾元素永远不可能成为窗口最大值，弹出
            while (!deque.isEmpty() && nums[i] >= nums[deque.peekLast()]) {
                deque.pollLast();
            }
            deque.offerLast(i);
        }

        int[] ans = new int[n - k + 1];
        // 第一个窗口的最大值即为队头元素
        ans[0] = nums[deque.peekFirst()];
        
        // 2. 滑动窗口：从第 k 个元素开始遍历
        for (int i = k; i < n; ++i) {
            // 保持单调递减性质：如果新元素大于等于队尾元素，弹出队尾
            while (!deque.isEmpty() && nums[i] >= nums[deque.peekLast()]) {
                deque.pollLast();
            }
            deque.offerLast(i);
            
            // 移除过期元素：如果队头索引已经不在当前窗口内 [i-k+1, i]，则弹出队头
            while (deque.peekFirst() <= i - k) {
                deque.pollFirst();
            }
            
            // 当前窗口的最大值即为队头元素
            ans[i - k + 1] = nums[deque.peekFirst()];
        }
        return ans;
    }
}


// 解法二：分块 + 预处理（前缀/后缀最大值）
class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        int n = nums.length;
        // prefix[i] 表示从当前块的开始到索引 i 的最大值
        // suffix[i] 表示从索引 i 到当前块的结束的最大值
        int[] prefix = new int[n], suffix = new int[n];
        
        // 1. 分块计算前缀最大值
        for (int i = 0; i < n; ++i) {
            if (i % k == 0) {
                // 如果是块的起始位置，重新开始记录最大值
                prefix[i] = nums[i];
            } else {
                prefix[i] = Math.max(prefix[i - 1], nums[i]);
            }
        }
        
        // 2. 分块计算后缀最大值
        for (int i = n - 1; 0 <= i; --i) {
            if (i == n - 1 || (i + 1) % k == 0) {
                // 如果是块的末尾位置（或者是数组最后一个元素），重新开始记录最大值
                suffix[i] = nums[i];
            } else {
                suffix[i] = Math.max(suffix[i + 1], nums[i]);
            }
        }
        
        int[] ans = new int[n - k + 1];
        // 3. 计算结果：任意一个长度为 k 的窗口 [i, i+k-1] 都会被块边界分成两部分
        // 窗口最大值 = max(当前窗口跨越的第一个块的后缀最大值, 第二个块的前缀最大值)
        for (int i = 0; i <= n - k; ++i) {
            ans[i] = Math.max(suffix[i], prefix[i + k - 1]);
        }
        return ans;
    }
}

```
