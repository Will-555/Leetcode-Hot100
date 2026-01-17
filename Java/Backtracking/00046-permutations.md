给定一个不含重复数字的数组 `nums` ，返回其 **所有可能的全排列** 。你可以 **按任意顺序** 返回答案。

**示例 1：**
输入：`nums = [1,2,3]`
输出：`[[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]`

**示例 2：**
输入：`nums = [0,1]`
输出：`[[0,1],[1,0]]`

**示例 3：**
输入：`nums = [1]`
输出：`[[1]]`

**提示：**
- `1 <= nums.length <= 6`
- `-10 <= nums[i] <= 10`
- `nums` 中的所有整数 **互不相同**

```Java
class Solution {
    public List<List<Integer>> permute(int[] nums) {
        List<List<Integer>> res = new ArrayList<List<Integer>>();
        List<Integer> output = new ArrayList<Integer>();
        for (int num : nums) {
            output.add(num);
        }
        int n = nums.length;
        backtrack(n, output, res, 0);
        return res;
    }

    public void backtrack(int n, List<Integer> output, List<List<Integer>> res, int first) {
        if (first == n) {
            res.add(new ArrayList<Integer>(output));
        }
        for (int i = first; i < n; i++) {
            Collections.swap(output, first, i);
            backtrack(n, output, res, first + 1);
            Collections.swap(output, first, i);
        }
    }
}
```
