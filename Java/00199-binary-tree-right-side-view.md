```Java
class Solution {
    public List<Integer> rightSideView(TreeNode root) {
        List<Integer> ans = new ArrayList<>();
        if (root == null) return ans;
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        while (0 < queue.size()) {
            int size = queue.size();
            while (0 < size--) {
                root = queue.poll();
                if (root.left != null) queue.offer(root.left);
                if (root.right != null) queue.offer(root.right);
            }
            ans.add(root.val);
        }
        return ans;
    }
}
```

```Java
import java.util.ArrayList;

class Solution {

    public List<Integer> rightSideView(TreeNode root) {
        List<Integer> ans = new ArrayList<>();
        _helper(root, ans, 0);
        return ans;
    }


    private void _helper(TreeNode node, List<Integer> res, int depth) {
        if (node == null) return;
        if (res.size() == depth) res.add(node.val);
        _helper(node.right, res, depth + 1);
        _helper(node.left, res, depth + 1);
    }
}
```