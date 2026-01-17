# 难度：中等

给你一个 `m` 行 `n` 列的矩阵 `matrix` ，请按照 **顺时针螺旋顺序** ，返回矩阵中的所有元素。

**示例 1：**
![Spiral Matrix 1](https://assets.leetcode.com/uploads/2020/11/13/spiral1.jpg)
输入：`matrix = [[1,2,3],[4,5,6],[7,8,9]]`
输出：`[1,2,3,6,9,8,7,4,5]`

**示例 2：**
![Spiral Matrix 2](https://assets.leetcode.com/uploads/2020/11/13/spiral.jpg)
输入：`matrix = [[1,2,3,4],[5,6,7,8],[9,10,11,12]]`
输出：`[1,2,3,4,8,12,11,10,9,5,6,7]`

**提示：**
- `m == matrix.length`
- `n == matrix[i].length`
- `1 <= m, n <= 10`
- `-100 <= matrix[i][j] <= 100`

```Java
class Solution {
    public List<Integer> spiralOrder(int[][] matrix) {
        List<Integer> order = new ArrayList<Integer>();
        if (matrix == null || matrix.length == 0 || matrix[0].length == 0) {
            return order;
        }
        int rows = matrix.length, columns = matrix[0].length;
        int left = 0, right = columns - 1, top = 0, bottom = rows - 1;
        while (left <= right && top <= bottom) {
            for (int column = left; column <= right; column++) {
                order.add(matrix[top][column]);
            }
            for (int row = top + 1; row <= bottom; row++) {
                order.add(matrix[row][right]);
            }
            if (left < right && top < bottom) {
                for (int column = right - 1; column > left; column--) {
                    order.add(matrix[bottom][column]);
                }
                for (int row = bottom; row > top; row--) {
                    order.add(matrix[row][left]);
                }
            }
            left++;
            right--;
            top++;
            bottom--;
        }
        return order;
    }
}
```
