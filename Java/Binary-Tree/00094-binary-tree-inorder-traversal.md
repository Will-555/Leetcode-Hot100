# 难度：简单

给你二叉树的根节点 `root` ，返回它节点值的 **中序** 遍历。

**示例 1：**
![Binary Tree Inorder 1](https://assets.leetcode.com/uploads/2020/09/15/inorder_1.jpg)
输入：`root = [1,null,2,3]`
输出：`[1,3,2]`

**示例 2：**
输入：`root = []`
输出：`[]`

**示例 3：**
输入：`root = [1]`
输出：`[1]`

**提示：**
- 树中节点数目在范围 `[0, 100]` 内
- `-100 <= Node.val <= 100`

```Java
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<Integer>();
        inorder(root, res);
        return res;
    }

    public void inorder(TreeNode root, List<Integer> res) {
        if (root == null) {
            return;
        }
        inorder(root.left, res);
        res.add(root.val);
        inorder(root.right, res);
    }
}
```
