# 难度：中等

给你二叉树的根节点 `root` ，请你将它展开为一个单链表：
- 展开后的单链表应该同样使用 `TreeNode` ，其中 `right` 子指针指向链表中下一个节点，而左子指针始终为 `null` 。
- 展开后的单链表应该与二叉树 [**先序遍历**](https://baike.baidu.com/item/%E5%85%88%E5%BA%8F%E9%81%8D%E5%8E%86/6442831?fromtitle=%E5%89%8D%E5%BA%8F%E9%81%8D%E5%8E%86&fromid=1873149) 顺序相同。

**示例 1：**
![Flatten Binary Tree 1](https://assets.leetcode.com/uploads/2021/01/14/flattentree.jpg)
输入：`root = [1,2,5,3,4,null,6]`
输出：`[1,null,2,null,3,null,4,null,5,null,6]`

**示例 2：**
输入：`root = []`
输出：`[]`

**示例 3：**
输入：`root = [0]`
输出：`[0]`

**提示：**
- 树中节点数在范围 `[0, 2000]` 内
- `-100 <= Node.val <= 100`

```Java
class Solution {
    public void flatten(TreeNode root) {
        TreeNode curr = root;
        while (curr != null) {
            if (curr.left != null) {
                TreeNode next = curr.left;
                TreeNode predecessor = next;
                while (predecessor.right != null) {
                    predecessor = predecessor.right;
                }
                predecessor.right = curr.right;
                curr.left = null;
                curr.right = next;
            }
            curr = curr.right;
        }
    }
}
```