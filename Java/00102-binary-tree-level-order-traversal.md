```Java
class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> ans = new ArrayList<>();
        if (root == null) return ans;
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        while (0 < queue.size()) {
            int size = queue.size();
            List<Integer> curList = new ArrayList<>();
            while (0 < size--) {
                TreeNode curNode = queue.remove();
                curList.add(curNode.val);
                if (curNode.left != null) queue.offer(curNode.left);
                if (curNode.right != null) queue.offer(curNode.right);
            }
            ans.add(curList);
        }
        return ans;
    }
}
```
