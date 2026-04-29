# 437. 路径总和 III (Path Sum III)

## 题目描述
给定一个二叉树，它的每个节点都包含一个整数值。找出所有路径的和等于给定目标值 `targetSum` 的路径数量。路径可以从任意节点开始，向下走到任意节点结束（不一定从根节点开始，也不一定到叶子节点结束），但路径必须连续且只能向下（父节点到子节点）。

**示例**
```
输入: root = [10,5,-3,3,2,null,11,3,-2,null,1], targetSum = 8
输出: 3
解释: 路径分别是:
1. 5 -> 3
2. 5 -> 2 -> 1
3. -3 -> 11
```

## 解题思路（前缀和 + DFS）
- 使用深度优先遍历（DFS）遍历树的每个节点，同时维护从根节点到当前节点的路径前缀和 `currSum`。
- 使用哈希表 `prefixCount` 记录每个前缀和出现的次数，初始化时 `prefixCount[0] = 1`（表示空路径）。
- 对于当前节点，若存在 `currSum - targetSum` 在哈希表中，则说明在当前路径上有若干段路径的和等于 `targetSum`，将对应次数累加到答案中。
- 递归遍历左、右子树后，需要回溯：将当前前缀和的计数减一，以免影响其他分支的计数。
- 该方法时间复杂度 `O(N)`（每个节点访问一次），空间复杂度 `O(H)`（递归栈深度）+ 哈希表大小 `O(N)` 最坏情况。

## 代码实现（Java）
```java
/**
 * Definition for a binary tree node.
 */
class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;
    TreeNode(int x) { val = x; }
}

class Solution {
    private long target;
    private int ans = 0;
    // 使用 Long 作为前缀和的键，防止整数溢出
    private Map<Long, Integer> prefixCount = new HashMap<>();

    public int pathSum(TreeNode root, int targetSum) {
        this.target = targetSum;
        prefixCount.put(0L, 1); // 空路径的前缀和为 0
        dfs(root, 0L);
        return ans;
    }

    private void dfs(TreeNode node, long currSum) {
        if (node == null) return;
        // 更新当前前缀和（使用 long 防止溢出）
        currSum += node.val;
        // 检查是否存在满足条件的前缀和
        ans += prefixCount.getOrDefault(currSum - target, 0);
        // 将当前前缀和加入哈希表
        prefixCount.put(currSum, prefixCount.getOrDefault(currSum, 0) + 1);
        // 继续遍历左右子树
        dfs(node.left, currSum);
        dfs(node.right, currSum);
        // 回溯：移除当前前缀和的计数
        prefixCount.put(currSum, prefixCount.get(currSum) - 1);
    }
}
```

## 复杂度分析
- **时间复杂度**：`O(N)`，其中 `N` 为二叉树节点数，每个节点仅访问一次。
- **空间复杂度**：`O(N)`，哈希表最多存储 `N` 个不同的前缀和；递归栈深度为树的高度 `H`，最坏情况下 `O(N)`。
