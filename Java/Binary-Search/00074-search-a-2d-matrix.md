给你一个满足下述两条属性的 `m x n` 整数矩阵：

- 每行中的整数从左到右按非严格递增顺序排列。
- 每行的第一个整数大于前一行的最后一个整数。

给你一个整数 `target` ，如果 `target` 在矩阵中，返回 `true` ；否则，返回 `false` 。

**示例 1：**
![Matrix 1](https://assets.leetcode.com/uploads/2020/10/05/mat.jpg)
输入：`matrix = [[1,3,5,7],[10,11,16,20],[23,30,34,60]], target = 3`
输出：`true`

**示例 2：**
![Matrix 2](https://assets.leetcode.com/uploads/2020/10/05/mat2.jpg)
输入：`matrix = [[1,3,5,7],[10,11,16,20],[23,30,34,60]], target = 13`
输出：`false`

**提示：**
- `m == matrix.length`
- `n == matrix[i].length`
- `1 <= m, n <= 100`
- `-10^4 <= matrix[i][j], target <= 10^4`

```Java
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        int m = matrix.length, n = matrix[0].length;
        int low = 0, high = m * n - 1;
        while (low <= high) {
            int mid = (high - low) / 2 + low;
            int x = matrix[mid / n][mid % n];
            if (x < target) {
                low = mid + 1;
            } else if (x > target) {
                high = mid - 1;
            } else {
                return true;
            }
        }
        return false;
    }
}
```
