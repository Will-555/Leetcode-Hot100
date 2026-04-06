给定一个包含 `n + 1` 个整数的数组 `nums` ，其数字都在 `[1, n]` 范围内（包括 `1` 和 `n`），可知至少存在一个重复的整数。

假设 `nums` 只有 **一个** 重复的整数，返回 **这个重复的数** 。

你设计的解决方案必须 **不修改** 数组 `nums` 且只用常量级 `O(1)` 的额外空间。

**示例 1：**
输入：`nums = [1,3,4,2,2]`
输出：`2`

**示例 2：**
输入：`nums = [3,1,3,4,2]`
输出：`3`

**示例 3：**
输入：`nums = [3,3,3,3,3]`
输出：`3`

**提示：**
- `1 <= n <= 10^5`
- `nums.length == n + 1`
- `1 <= nums[i] <= n`
- `nums` 中只有一个整数出现两次或多次，其余整数均只出现一次

**进阶：**
- 如何证明 `nums` 中至少存在一个重复的数字?
- 你可以设计一个线性级时间复杂度 `O(n)` 的解决方案吗？

```Java
class Solution {
    public int findDuplicate(int[] nums) {
        // 使用快慢指针法（Floyd 判圈算法）
        // 因为 nums 中的数字都在 1 到 n 之间，可以把 nums[i] 看作是从 i 指向 nums[i] 的边
        // 这样就构成了一个链表。因为有重复数字，所以一定有环。
        // 题目就转化为了求环的入口节点。

        int slow = 0;
        int fast = 0;
        
        // 第一阶段：寻找相遇点
        do {
            slow = nums[slow];
            fast = nums[nums[fast]];
        } while (slow != fast);

        // 第二阶段：寻找环入口
        slow = 0;
        while (slow != fast) {
            slow = nums[slow];
            fast = nums[fast];
        }

        return slow;
    }
}
```
