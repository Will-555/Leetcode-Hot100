```Java
// 官方题解 https://leetcode.cn/problems/longest-consecutive-sequence/solutions/276931/zui-chang-lian-xu-xu-lie-by-leetcode-solution/
class Solution {
    public int longestConsecutive(int[] nums) {
        int ans = 0;
        Set<Integer> set = new HashSet<>();
        for (int num : nums) set.add(num);
        for (int num : set) {
            if (!set.contains(num - 1)) {
                int curNum = num;
                int curSize = 1;
                while (set.contains(curNum + 1)) {
                    curSize++;
                    curNum++;
                }
                ans = Math.max(ans, curSize);
            }
        }
        return ans;
    }
}
```

```Java
class Solution {
    public int longestConsecutive(int[] nums) {
        int ans = 0;
        Set<Integer> set = new HashSet<>();
        for (int num : nums) set.add(num);
        for (int num : nums) {
            int tempNum = num, count = 1;
            while (set.contains(--tempNum)) {
                count++;
                set.remove(tempNum);
            }
            tempNum = num;
            while (set.contains(++tempNum)) {
                count++;
                set.remove(tempNum);
            }
            ans = Math.max(ans, count);
        }
        return ans;
    }
}
```