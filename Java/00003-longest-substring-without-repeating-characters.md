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
