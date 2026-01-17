```Java

class Solution {

    public TreeNode sortedArrayToBST(int[] nums) {
        return _helper(nums, 0, nums.length - 1);
    }

    private TreeNode _helper(int[] nums, int start, int end) {
        if (end < start) return null;
        int mid = start + (end - start) / 2;
        TreeNode root = new TreeNode(nums[mid]);
        root.left = _helper(nums, start, mid - 1);
        root.right = _helper(nums, mid + 1, end);
        return root;
    }

}
```
