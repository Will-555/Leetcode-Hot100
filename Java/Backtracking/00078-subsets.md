# 难度：中等

给你一个整数数组 `nums` ，数组中的元素 **互不相同** 。返回该数组所有可能的子集（幂集）。

解集 **不能** 包含重复的子集。你可以按 **任意顺序** 返回解集。

**示例 1：**
输入：`nums = [1,2,3]`
输出：`[[],[1],[2],[1,2],[3],[1,3],[2,3],[1,2,3]]`

**示例 2：**
输入：`nums = [0]`
输出：`[[],[0]]`

**提示：**
- `1 <= nums.length <= 10`
- `-10 <= nums[i] <= 10`
- `nums` 中的所有元素 **互不相同**

```Java
class Solution {
    List<Integer> t = new ArrayList<Integer>();
    List<List<Integer>> ans = new ArrayList<List<Integer>>();

    public List<List<Integer>> subsets(int[] nums) {
        backtrack(0, nums);
        return ans;
    }

    public void backtrack(int cur, int[] nums) {
        if (cur == nums.length) {
            ans.add(new ArrayList<Integer>(t));
            return;
        }
        t.add(nums[cur]);
        backtrack(cur + 1, nums);
        t.remove(t.size() - 1);
        backtrack(cur + 1, nums);
    }
}
```
