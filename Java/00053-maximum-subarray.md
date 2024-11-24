```Java
class Solution {
    public int maxSubArray(int[] nums) {
        int ans = -Integer.MAX_VALUE, curSum = 0;
        for (int i = 0; i < nums.length; ++i) {
            curSum = Math.max(curSum + nums[i], nums[i]);
            ans = Math.max(ans, curSum);
        }
        return ans;
    }
}
```
