[官方题解](https://leetcode.cn/problems/validate-binary-search-tree/?envType=study-plan-v2&envId=top-100-liked)
```Java
class Solution {
    public boolean isValidBST(TreeNode root) {
        if (root == null) return true;
        Stack<TreeNode> stack = new Stack<>();
        TreeNode pre = null;
        while (root != null || !stack.isEmpty()) {
            while (root != null) {
                stack.push(root);
                root = root.left;
            }
            root = stack.pop();
            if (pre != null && root.val <= pre.val) return false;
            pre = root;
            root = root.right;
        }
        return true;
    }
}
```

```Java
class Solution {
    public boolean isValidBST(TreeNode root) {
        return _helper(root, Long.MIN_VALUE, Long.MAX_VALUE);
    }
    
    private boolean _helper(TreeNode root, int small, int big) {
        if (root == null) return true;
        if (root.val <= small || root.val >= big) return false;
        return _helper(root.left, small, root.val) && _helper(root.right, root.val, big);
    }
}
```
