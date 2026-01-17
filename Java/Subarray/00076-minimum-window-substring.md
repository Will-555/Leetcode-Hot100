给定两个字符串 s 和 t，长度分别是 m 和 n，返回 s 中的 最短窗口 子串，使得该子串包含 t 中的每一个字符（包括重复字符）。如果没有这样的子串，返回空字符串 ""。

测试用例保证答案唯一。

示例 1：
输入：s = "ADOBECODEBANC", t = "ABC"
输出："BANC"
解释：最小覆盖子串 "BANC" 包含来自字符串 t 的 'A'、'B' 和 'C'。

示例 2：
输入：s = "a", t = "a"
输出："a"
解释：整个字符串 s 是最小覆盖子串。

示例 3:
输入: s = "a", t = "aa"
输出: ""
解释: t 中两个字符 'a' 均应包含在 s 的子串中，
因此没有符合条件的子字符串，返回空字符串。

提示：

m == s.length
n == t.length
1 <= m, n <= 105
s 和 t 由英文字母组成

```Java
class Solution {
    public String minWindow(String s, String t) {
        int sLen = s.length(), tLen = t.length();
        if (sLen < tLen) return "";
        int minStart = 0, curStart = 0;
        int count = tLen, minLen = Integer.MAX_VALUE;
        int[] map = new int[128];
        for (int i = 0; i < tLen; ++i) map[t.charAt(i)]++;
        for (int i = 0; i < sLen; ++i) {
            if (0 < map[s.charAt(i)]) --count;
            --map[s.charAt(i)];
            while (0 == count) {
                int curLen = i - curStart + 1;
                if (curLen < minLen) {
                    minStart = curStart;
                    minLen = curLen;
                }
                map[s.charAt(curStart)]++;
                if (0 < map[s.charAt(curStart)]) count++;
                ++curStart;
            }
        }
        if (sLen < minLen + minStart) return "";
        return s.substring(minStart, minStart + minLen);
    }
}
```
