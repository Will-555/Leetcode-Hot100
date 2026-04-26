# 236. 二叉树的最近公共祖先 (Lowest Common Ancestor of a Binary Tree)

[LeetCode 题目链接](https://leetcode.cn/problems/lowest-common-ancestor-of-a-binary-tree/)

## 题目描述

给定一个二叉树, 找到该树中两个指定节点的最近公共祖先 (LCA)。

最近公共祖先的定义为：“对于有根树 T 的两个节点 p、q，最近公共祖先表示为一个节点 x，满足 x 是 p、q 的祖先且 x 的深度尽可能大（一个节点也可以是它自己的祖先）。”

## 解题思路

寻找最近公共祖先通常使用 **后序遍历 (Recursive DFS)**。

### 递归逻辑：
1.  **终止条件：**
    *   如果当前节点 `root` 为空，返回 `null`。
    *   如果当前节点 `root` 就是 `p` 或 `q`，直接返回 `root`。
2.  **递归寻找：**
    *   在左子树中寻找 `p` 或 `q`，结果记为 `left`。
    *   在右子树中寻找 `p` 或 `q`，结果记为 `right`。
3.  **结果判断：**
    *   若 `left` 和 `right` 都不为空：说明 `p` 和 `q` 分别分布在当前节点的左右子树中，则当前节点 `root` 就是最近公共祖先。
    *   若 `left` 为空且 `right` 不为空：说明 `p` 和 `q` 都在右子树中，返回 `right`。
    *   若 `right` 为空且 `left` 不为空：说明 `p` 和 `q` 都在左子树中，返回 `left`。
    *   若都为空：返回 `null`。

## 代码实现

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        // 1. 终止条件：找到 p 或 q，或者遍历到了叶子节点之后
        if (root == null || root == p || root == q) {
            return root;
        }

        // 2. 递归在左右子树中寻找
        TreeNode left = lowestCommonAncestor(root.left, p, q);
        TreeNode right = lowestCommonAncestor(root.right, p, q);

        // 3. 结果合并与判断
        if (left != null && right != null) {
            // 左右子树各找到一个，说明当前节点是祖先
            return root;
        }
        
        // 谁不为空返回谁（如果都为空则返回 null）
        return left != null ? left : right;
    }
}
```

## 复杂度分析

*   **时间复杂度：** $O(N)$，其中 $N$ 是二叉树的节点数。最坏情况下需要遍历所有节点。
*   **空间复杂度：** $O(N)$，最坏情况下（树退化为链表）递归栈深度为 $N$。
