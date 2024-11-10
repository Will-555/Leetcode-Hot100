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
