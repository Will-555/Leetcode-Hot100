# 难度：中等

给定一个二叉树的根节点 `root`，想象自己站在它的右侧，按照从顶部到底部的顺序，返回从右侧所能看到的节点值。

**示例 1:**
![Binary Tree Right Side View 1](https://assets.leetcode.com/uploads/2021/02/14/tree.jpg)
输入: `[1,2,3,null,5,null,4]`
输出: `[1,3,4]`

**示例 2:**
输入: `[1,null,3]`
输出: `[1,3]`

**示例 3:**
输入: `[]`
输出: `[]`

**提示:**
- 二叉树的节点个数的范围是 `[0,100]`
- `-100 <= Node.val <= 100` 

```Java
class Solution {
    public List<Integer> rightSideView(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        if (root == null) {
            return res;
        }
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        while (!queue.isEmpty()) {
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                TreeNode node = queue.poll();
                if (node.left != null) {
                    queue.offer(node.left);
                }
                if (node.right != null) {
                    queue.offer(node.right);
                }
                if (i == size - 1) {  // 如果是当前层的最后一个节点
                    res.add(node.val);
                }
            }
        }
        return res;
    }
}
```