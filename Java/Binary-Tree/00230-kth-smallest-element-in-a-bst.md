[官方题解](https://leetcode.cn/problems/kth-smallest-element-in-a-bst/solutions/1050055/er-cha-sou-suo-shu-zhong-di-kxiao-de-yua-8o07/?envType=study-plan-v2&envId=top-100-liked)
```Java
class Solution {

    private int k = 0;
    private int ans;
    public int kthSmallest(TreeNode root, int k) {
        this.k = k;
        _helper(root);
        return ans;
    }

    private void _helper(TreeNode node) {
        if (node == null) return;
        _helper(node.left);
        k--;
        if (k == 0) {
            ans = node.val;
            return;
        }
        _helper(node.right);
    }
}
```


```Java
class Solution {

    public int kthSmallest(TreeNode root, int k) {
        Stack<TreeNode> stack = new Stack<>();
        while (root != null || !stack.isEmpty()) {
            while (root != null) {
                stack.push(root);
                root = root.left;
            }
            root = stack.pop();
            --k;
            if (k == 0) return root.val;
            root = root.right;
        }
        return root.val;
    }
}
```