# 难度：中等

给定一个二叉搜索树的根节点 `root` ，和一个整数 `k` ，请你设计一个算法查找其中第 `k` 个最小元素（从 1 开始计数）。

**示例 1：**
![BST Kth Smallest 1](https://assets.leetcode.com/uploads/2021/01/28/kthtree1.jpg)
输入：`root = [3,1,4,null,2], k = 1`
输出：`1`

**示例 2：**
![BST Kth Smallest 2](https://assets.leetcode.com/uploads/2021/01/28/kthtree2.jpg)
输入：`root = [5,3,6,2,4,null,null,1], k = 3`
输出：`3`

**提示：**
- 树中的节点数为 `n` 
- `1 <= k <= n <= 10^4`
- `0 <= Node.val <= 10^4`

```Java
class Solution {
    public int kthSmallest(TreeNode root, int k) {
        Deque<TreeNode> stack = new ArrayDeque<TreeNode>();
        while (root != null || !stack.isEmpty()) {
            while (root != null) {
                stack.push(root);
                root = root.left;
            }
            root = stack.pop();
            --k;
            if (k == 0) {
                break;
            }
            root = root.right;
        }
        return root.val;
    }
}
```