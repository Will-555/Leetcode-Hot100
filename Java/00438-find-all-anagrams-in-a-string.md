给定两个字符串 s 和 p，找到 s 中所有 p 的 异位词 的子串，返回这些子串的起始索引。不考虑答案输出的顺序。

示例 1:

输入: s = "cbaebabacd", p = "abc"
输出: [0,6]
解释:
起始索引等于 0 的子串是 "cba", 它是 "abc" 的异位词。
起始索引等于 6 的子串是 "bac", 它是 "abc" 的异位词。
 示例 2:

输入: s = "abab", p = "ab"
输出: [0,1,2]
解释:
起始索引等于 0 的子串是 "ab", 它是 "ab" 的异位词。
起始索引等于 1 的子串是 "ba", 它是 "ab" 的异位词。
起始索引等于 2 的子串是 "ab", 它是 "ab" 的异位词。


[官方题解](https://leetcode.cn/problems/find-all-anagrams-in-a-string/solutions/1123971/zhao-dao-zi-fu-chuan-zhong-suo-you-zi-mu-xzin/?envType=study-plan-v2&envId=top-100-liked)  
```Java
class Solution {
    public List<Integer> findAnagrams(String s, String p) {
        int[] sArray = new int[26], pArray = new int[26];
        List<Integer> ans = new ArrayList<>();
        int sLen = s.length(), pLen = p.length();
        if (sLen < pLen) return ans;
        for (int i = 0; i < pLen; ++i) {
            sArray[s.charAt(i) - 'a']++;
            pArray[p.charAt(i) - 'a']++;
        }
        int start = 0, end = pLen;
        if (Arrays.equals(sArray, pArray)) ans.add(start);
        while (end < sLen) {
            sArray[s.charAt(start++) - 'a']--;
            sArray[s.charAt(end++) - 'a']++;
            if (Arrays.equals(sArray, pArray)) ans.add(start);
        }
        return ans;
    }
      
}
```
