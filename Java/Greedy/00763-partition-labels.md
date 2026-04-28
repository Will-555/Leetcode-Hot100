# 难度：中等

## 题目描述
给定一个只包含小写字母的字符串 `s`，将其划分为尽可能多的片段，使得每个字母只出现在一个片段中。返回每个片段的长度列表。

**示例 1**
```
输入: s = "ababcbacadefegdehijhklij"
输出: [9,7,8]
解释: 划分结果为 "ababcbaca", "defegde", "hijhklij"。
```

**示例 2**
```
输入: s = "eccbbbbdec"
输出: [10]
```

**提示**
- `1 <= s.length <= 500`
- `s` 仅由小写英文字母组成。

## 解题思路（贪心）
- 首先遍历字符串，记录每个字符最后出现的位置 `last[char]`。
- 再次遍历时维护当前片段的最右边界 `end`，取遍历到的字符的 `last` 与 `end` 的最大值。
- 当遍历下标 `i` 等于 `end` 时，说明当前片段可以结束，记录长度并开启新片段。
- 该过程只需一次遍历，符合贪心策略：尽可能早地结束片段，以获得最多的片段数。

## 代码实现（Java）
```java
import java.util.*;

class Solution {
    /**
     * 将字符串划分为若干片段，使得每个字母只出现于一个片段中，返回各片段长度。
     */
    public List<Integer> partitionLabels(String s) {
        // 记录每个字符最后出现的位置
        int[] last = new int[26];
        for (int i = 0; i < s.length(); i++) {
            last[s.charAt(i) - 'a'] = i;
        }
        List<Integer> res = new ArrayList<>();
        int start = 0, end = 0;
        for (int i = 0; i < s.length(); i++) {
            // 更新当前片段的最右边界
            end = Math.max(end, last[s.charAt(i) - 'a']);
            // 当遍历到片段右边界时，结束当前片段
            if (i == end) {
                res.add(end - start + 1);
                start = i + 1; // 开启新片段
            }
        }
        return res;
    }
}
```

## 复杂度分析
- 时间复杂度：`O(n)`，只遍历字符串两遍。
- 空间复杂度：`O(1)`，使用固定大小的数组记录字符位置，返回列表额外 `O(k)`（`k` 为片段数）。
