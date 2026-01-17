# 难度：简单

给你一个二叉树的根节点 `root` ， 检查它是否轴对称。

**示例 1：**
![Symmetric Tree 1](https://assets.leetcode.com/uploads/2021/02/19/symtree1.jpg)
输入：`root = [1,2,2,3,4,4,3]`
输出：`true`

**示例 2：**
![Symmetric Tree 2](https://assets.leetcode.com/uploads/2021/02/19/symtree2.jpg)
输入：`root = [1,2,2,null,3,null,3]`
输出：`false`

**提示：**
- 树中节点数目在范围 `[1, 1000]` 内
- `-100 <= Node.val <= 100`

```Java
class Solution {
    public boolean isSymmetric(TreeNode root) {
        return check(root, root);
    }

    public boolean check(TreeNode p, TreeNode q) {
        if (p == null && q == null) {
            return true;
        }
        if (p == null || q == null) {
            return false;
        }
        return p.val == q.val && check(p.left, q.right) && check(p.right, q.left);
    }
}
```
