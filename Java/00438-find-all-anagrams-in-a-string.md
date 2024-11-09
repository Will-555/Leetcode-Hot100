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
