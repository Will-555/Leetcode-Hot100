```Java
class Solution {

    int ans = 0;
    
    public int diameterOfBinaryTree(TreeNode root) {
        helper(root);
        return ans;
    }
    
    private int helper(TreeNode root) {
        if (root == null) return 0;
        int left = helper(root.left);
        int right = helper(root.right);
        ans = Math.max(ans, left + right);
        return Math.max(left, right) + 1;
    }
}
```
