# 难度：中等

给你一个字符串 `s`，找到 `s` 中最长的回文子串。

如果字符串的反序与原始字符串相同，则该字符串称为回文字符串。

**示例 1：**
输入：`s = "babad"`
输出：`"bab"`
解释：`"aba"` 同样是符合题意的答案。

**示例 2：**
输入：`s = "cbbd"`
输出：`"bb"`

**提示：**
- `1 <= s.length <= 1000`
- `s` 仅由数字和英文字母组成

```Java
class Solution {
    public String longestPalindrome(String s) {
        if (s == null || s.length() < 1) {
            return "";
        }
        int start = 0, end = 0;
        for (int i = 0; i < s.length(); i++) {
            // 以 i 为中心扩展（奇数长度）
            int len1 = expandAroundCenter(s, i, i);
            // 以 i 和 i+1 为中心扩展（偶数长度）
            int len2 = expandAroundCenter(s, i, i + 1);
            int len = Math.max(len1, len2);
            if (len > end - start) {
                start = i - (len - 1) / 2;
                end = i + len / 2;
            }
        }
        return s.substring(start, end + 1);
    }

    private int expandAroundCenter(String s, int left, int right) {
        while (left >= 0 && right < s.length() && s.charAt(left) == s.charAt(right)) {
            left--;
            right++;
        }
        // 返回回文串长度
        return right - left - 1;
    }
}
```
