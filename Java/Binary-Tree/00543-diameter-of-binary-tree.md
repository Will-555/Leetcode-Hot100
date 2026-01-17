# 难度：简单

给你一棵二叉树的根节点，返回该树的 **直径** 。

二叉树的 **直径** 是指树中任意两个节点之间路径的长度。这条路径可能经过也可能不经过根节点 `root` 。

两节点之间路径的 **长度** 是由它们之间边数表示的。

**示例 1：**
![Diameter of Binary Tree 1](https://assets.leetcode.com/uploads/2021/03/06/diamtree.jpg)
输入：`root = [1,2,3,4,5]`
输出：`3`
解释：3 是从 [4,2,1,3] 或 [5,2,1,3] 的长度。

**示例 2：**
输入：`root = [1,2]`
输出：`1`

**提示：**
- 树中节点数目在范围 `[1, 10^4]` 内
- `-100 <= Node.val <= 100`

```Java
class Solution {
    int ans;
    public int diameterOfBinaryTree(TreeNode root) {
        ans = 1;
        depth(root);
        return ans - 1;
    }
    public int depth(TreeNode node) {
        if (node == null) {
            return 0; // 访问到空节点了，返回0
        }
        int L = depth(node.left); // 左儿子为根的子树的深度
        int R = depth(node.right); // 右儿子为根的子树的深度
        ans = Math.max(ans, L + R + 1); // 计算d_node即L+R+1 并更新ans
        return Math.max(L, R) + 1; // 返回该节点为根的子树的深度
    }
}
```
