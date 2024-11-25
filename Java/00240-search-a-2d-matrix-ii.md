```Java
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        int m = matrix.length - 1, n = matrix[0].length - 1;
        int left = n, top = 0;
        while (0 <= left && top <= m) {
            if (matrix[top][left] == target) return true;
            if (matrix[top][left] < target) top++;
            else left--;
        }
        return false;
    }
}
```
