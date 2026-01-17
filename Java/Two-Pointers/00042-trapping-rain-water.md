```Java
class Solution {
    public int trap(int[] height) {
        int n = height.length;
        if (n < 3) return 0;
        int maxLeftH = 0, maxRightH = 0;
        int ans = 0, left = 0, right = n - 1;
        while (left < right) {
            if (height[left] < height[right]) {
                if (maxLeftH < height[left]) maxLeftH = height[left];
                else ans += maxLeftH - height[left];
                ++left;
            } else {
                if (maxRightH < height[right]) maxRightH = height[right];
                else ans += maxRightH - height[right];
                --right;
            }
        }
        return ans;
    }
}
```
