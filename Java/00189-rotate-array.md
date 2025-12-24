## 189. 轮转数组
给定一个整数数组 nums，将数组中的元素向右轮转 k 个位置，其中 k 是非负数。


示例 1:
输入: nums = [1,2,3,4,5,6,7], k = 3
输出: [5,6,7,1,2,3,4]
解释:
向右轮转 1 步: [7,1,2,3,4,5,6]
向右轮转 2 步: [6,7,1,2,3,4,5]
向右轮转 3 步: [5,6,7,1,2,3,4]

示例 2:
输入：nums = [-1,-100,3,99], k = 2
输出：[3,99,-1,-100]
解释: 
向右轮转 1 步: [99,-1,-100,3]
向右轮转 2 步: [3,99,-1,-100]
 
提示：
1 <= nums.length <= 105
-231 <= nums[i] <= 231 - 1
0 <= k <= 105

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
