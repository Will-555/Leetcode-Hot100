[官方题解](https://leetcode.cn/problems/move-zeroes/solutions/489622/yi-dong-ling-by-leetcode-solution/)  

```Java
class Solution {
    public void moveZeroes(int[] nums) {
        int leftIndex = 0;
        for (int i = 0; i < nums.length; ++i) {
            if (nums[i] != 0) {
                int temp = nums[i];
                nums[i] = 0;
                nums[leftIndex++] = temp;
            }
        }
    }
}
```
