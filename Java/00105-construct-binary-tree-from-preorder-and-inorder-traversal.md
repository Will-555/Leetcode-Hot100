```Java
class Solution {
    public TreeNode buildTree(int[] preorder, int[] inorder) {
        return _helper(preorder, 0, preorder.length - 1, inorder, 0, inorder.length - 1);
    }

    public TreeNode _helper(int[] preorder, int preStart, int preEnd, int[] inorder, int inStart, int inEnd) {
        if (preEnd < preStart || inEnd < inStart) return null;
        int rootVal = preorder[preStart];
        TreeNode root = new TreeNode(rootVal);
        int rootIndex = 0;
        for (int i = inStart; i <= inEnd; ++i) {
            if (rootVal == inorder[i]) {
                rootIndex = i;
                break;
            }
        }
        int left = rootIndex - inStart, right = inEnd - rootIndex;
        root.left = _helper(preorder, preStart + 1, preStart + left, inorder, inStart, rootIndex - 1);
        root.right = _helper(preorder, preStart + left + 1, preEnd, inorder, rootIndex + 1, inEnd);
        return root;
    }
}
```
