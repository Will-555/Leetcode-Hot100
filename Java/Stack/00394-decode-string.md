# 394. 字符串解码 (Decode String)

[LeetCode 题目链接](https://leetcode.cn/problems/decode-string/)

## 题目描述

给定一个经过编码的字符串，返回它解码后的字符串。

编码规则为: `k[encoded_string]`，表示其中方括号内部的 `encoded_string` 正好重复 `k` 次。注意 `k` 保证为正整数。

你可以认为输入字符串总是有效的；输入字符串中没有额外的空格，且输入的方括号总是符合格式要求的。

此外，你可以认为原始数据不包含数字，所有的数字只表示重复的次数 `k` ，例如不会出现像 `3a` 或 `2[4]` 的输入。

**示例：**
*   输入：`s = "3[a]2[bc]"` -> 输出：`"aaabcbc"`
*   输入：`s = "3[a2[c]]"` -> 输出：`"accaccacc"`

## 解题思路

由于括号存在嵌套关系，这符合 **后进先出** 的特性，因此使用 **栈 (Stack)** 是最直观的方法。

### 辅助栈法：
我们需要存储两个关键信息：执行当前括号内的重复次数 `multi`，以及之前已经拼接好的字符串 `last_res`。

1.  **准备工作：** 建立两个栈，`stack_multi` 存倍数，`stack_res` 存之前的字符串。
2.  **遍历字符串：**
    *   **如果是数字：** 累加计算完整的数字（处理多位数如 "100"）。
    *   **如果是 `[`：**
        *   将当前倍数放入 `stack_multi`。
        *   将当前已生成的字符串放入 `stack_res`。
        *   清空倍数和字符串，准备记录括号内的内容。
    *   **如果是 `]`：**
        *   从倍数栈弹出倍数 `cur_multi`。
        *   从结果栈弹出上一个状态的字符串 `last_res`。
        *   将当前记录的字符串（即括号内的内容）重复 `cur_multi` 次，并拼接在 `last_res` 后面。
    *   **如果是字母：** 直接拼接到当前字符串后面。

## 代码实现

```java
import java.util.LinkedList;

class Solution {
    public String decodeString(String s) {
        StringBuilder res = new StringBuilder();
        int multi = 0;
        // 使用 LinkedList 作为栈，性能较好
        LinkedList<Integer> stack_multi = new LinkedList<>();
        LinkedList<String> stack_res = new LinkedList<>();

        for (Character c : s.toCharArray()) {
            if (c == '[') {
                // 入栈当前状态
                stack_multi.addLast(multi);
                stack_res.addLast(res.toString());
                // 重置当前状态
                multi = 0;
                res = new StringBuilder();
            } else if (c == ']') {
                // 出栈并拼接
                StringBuilder tmp = new StringBuilder();
                int cur_multi = stack_multi.removeLast();
                for (int i = 0; i < cur_multi; i++) {
                    tmp.append(res);
                }
                res = new StringBuilder(stack_res.removeLast() + tmp);
            } else if (Character.isDigit(c)) {
                // 计算多位数字
                multi = multi * 10 + Integer.parseInt(c + "");
            } else {
                // 普通字符
                res.append(c);
            }
        }
        return res.toString();
    }
}
```

## 复杂度分析

*   **时间复杂度：** $O(S)$，其中 $S$ 是解码后字符串的长度。我们需要遍历原字符串，并根据重复次数构建新字符串。
*   **空间复杂度：** $O(S)$，栈的深度取决于括号嵌套深度，极端情况下空间与解码后的字符串长度相关。
