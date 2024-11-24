```Java
class Solution {
    public int[] productExceptSelf(int[] nums) {
        int len = nums.length;
        int[] ans = new int[len];
        Arrays.fill(ans, 1);
        int cur = 1;
        for (int i = 0; i < len; ++i) {
            ans[i] *= cur;
            cur *= nums[i];
        }
        cur = 1;
        for (int i = len - 1; 0 <= i; --i) {
            ans[i] *= cur;
            cur *= nums[i];
        }
        return ans;
    }
}
```

```Java
class Solution {
    public int[] productExceptSelf(int[] nums) {
        int len = nums.length;
        int[] ans = new int[len];
        ans[0] = nums[0];
        for (int i = 1; i < len; ++i) ans[i] = nums[i] * ans[i - 1];
        int res = 1;
        for (int i = len - 1; 0 < i; --i) {
            ans[i] = ans[i - 1] * res;
            res *= nums[i];
        }
        ans[0] = res;
        return ans;
    }
}
```
