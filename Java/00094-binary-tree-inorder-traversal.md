[官方题解](https://leetcode.cn/problems/binary-tree-inorder-traversal/solutions/412886/er-cha-shu-de-zhong-xu-bian-li-by-leetcode-solutio/?envType=study-plan-v2&envId=top-100-liked)  
```Java
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> ans = new ArrayList<>();
        inorder(ans, root);
        return ans;
    }

    private void inorder(List<Integer> list, TreeNode root) {
        if (root == null) return;
        inorder(list, root.left);
        list.add(root.val);
        inorder(list, root.right);
    }
}
```

```Java
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> ans = new ArrayList<Integer>();
        TreeNode predecessor = null;
        while (root != null) {
            if (root.left != null) {
                predecessor = root.left;
                while (predecessor.right != null && predecessor.right != root) {
                    predecessor = predecessor.right;
                }
                if (predecessor.right == null) {
                    predecessor.right = root;
                    root = root.left;
                } else {
                    ans.add(root.val);
                    predecessor.right = null;
                    root = root.right;
                }
            } else {
                ans.add(root.val);
                root = root.right;
            }
        }
        return ans;
    }
}
```
