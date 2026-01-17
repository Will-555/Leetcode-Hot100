# 难度：简单

给你一个整数数组 `nums` ，其中元素已经按 **升序** 排列，请你将其转换为一棵 **平衡** 二叉搜索树。

**示例 1：**
![Sorted Array to BST 1](https://assets.leetcode.com/uploads/2021/02/18/btree1.jpg)
输入：`nums = [-10,-3,0,5,9]`
输出：`[0,-3,9,-10,null,5]`
解释：`[0,-10,5,null,-3,null,9]` 也将被视为正确答案。

**示例 2：**
![Sorted Array to BST 2](https://assets.leetcode.com/uploads/2021/02/18/btree.jpg)
输入：`nums = [1,3]`
输出：`[3,1]`
解释：`[1,null,3]` 和 `[3,1]` 都是高度平衡二叉搜索树。

**提示：**
- `1 <= nums.length <= 10^4`
- `-10^4 <= nums[i] <= 10^4`
- `nums` 按 **严格递增** 顺序排列

```Java
class Solution {
    public TreeNode sortedArrayToBST(int[] nums) {
        return helper(nums, 0, nums.length - 1);
    }

    public TreeNode helper(int[] nums, int left, int right) {
        if (left > right) {
            return null;
        }

        // 总是选择中间位置左边的数字作为根节点
        int mid = (left + right) / 2;

        TreeNode root = new TreeNode(nums[mid]);
        root.left = helper(nums, left, mid - 1);
        root.right = helper(nums, mid + 1, right);
        return root;
    }
}
```
