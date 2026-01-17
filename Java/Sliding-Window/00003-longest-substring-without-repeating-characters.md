# 难度：中等

给定一个字符串 `s` ，请你找出其中不含有重复字符的 **最长子串** 的长度。

**示例 1:**
输入: `s = "abcabcbb"`
输出: `3` 
解释: 因为无重复字符的最长子串是 `"abc"`，所以其长度为 3。

**示例 2:**
输入: `s = "bbbbb"`
输出: `1`
解释: 因为无重复字符的最长子串是 `"b"`，所以其长度为 1。

**示例 3:**
输入: `s = "pwwkew"`
输出: `3`
解释: 因为无重复字符的最长子串是 `"wke"`，所以其长度为 3。
     请注意，你的答案必须是 **子串** 的长度，`"pwke"` 是一个 *子序列*，不是子串。

**提示：**
- `0 <= s.length <= 5 * 10^4`
- `s` 由英文字母、数字、符号和空格组成

```Java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        int ans = 0, left = -1;
        int[] map = new int[128];
        Arrays.fill(map, -1);
        for (int i = 0; i < s.length(); ++i) {
            char c = s.charAt(i);
            if (-1 < map[c]) left = Math.max(left, map[c]);
            map[c] = i;
            ans = Math.max(ans, i - left);
        }
        return ans;
    }
}
```
