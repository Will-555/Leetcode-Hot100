[官方题解](https://leetcode.cn/problems/flatten-binary-tree-to-linked-list/solutions/356853/er-cha-shu-zhan-kai-wei-lian-biao-by-leetcode-solu/?envType=study-plan-v2&envId=top-100-liked)  
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

```Java
class Solution {
    public void flatten(TreeNode root) {
            List<TreeNode> nodeList = new ArrayList<>();
            preorderTraversal(root, nodeList);
            int size = nodeList.size();
            for (int i = 1; i < size; ++i) {
                TreeNode prev = nodeList.get(i - 1), curr = nodeList.get(i);
                prev.left = null;
                prev.right = curr;
            }
        
    }

    public void preorderTraversal(TreeNode root, List<TreeNode> list) {
        if (root != null) {
            list.add(root);
            preorderTraversal(root.left, list);
            preorderTraversal(root.right, list);
        }
    }
}
```