[官方题解](https://leetcode.cn/problems/rotate-array/solutions/551039/xuan-zhuan-shu-zu-by-leetcode-solution-nipk/?envType=study-plan-v2&envId=top-100-liked)  
```Java
class Solution {
    public void rotate(int[] nums, int k) {
        int len = nums.length;
        int modK = k % len;
        if (k == 0) return;
        rotateArray(nums, 0, len - 1);
        rotateArray(nums, 0, modK - 1);
        rotateArray(nums, modK, len - 1);
    }

    private void rotateArray(int[] array, int i, int j) {
        while (i < j) {
            int temp = array[i];
            array[i++] = array[j];
            array[j--] = temp;
        }
    }
}
```
