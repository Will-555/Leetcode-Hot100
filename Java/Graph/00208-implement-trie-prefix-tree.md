# 208. 实现 Trie (前缀树) (Implement Trie (Prefix Tree))

[LeetCode 题目链接](https://leetcode.cn/problems/implement-trie-prefix-tree/)

## 题目描述

**Trie**（发音类似 "try"）或者说 **前缀树** 是一种树形数据结构，用于高效地存储和检索字符串数据集中的键。这一数据结构有相当多的应用情景，例如自动补完和拼写检查。

请你实现 Trie 类：
*   `Trie()` 初始化前缀树对象。
*   `void insert(String word)` 向前缀树中插入字符串 `word` 。
*   `boolean search(String word)` 如果字符串 `word` 在前缀树中，返回 `true`（即，在检索之前已经插入）；否则，返回 `false` 。
*   `boolean startsWith(String prefix)` 如果之前已经插入的字符串 `word` 的前缀之一为 `prefix` ，返回 `true` ；否则，返回 `false` 。

## 解题思路

前缀树的核心思想是 **空间换时间**，利用字符串的公共前缀来减少无谓的字符串比较。

### 节点结构 TrieNode：
1.  **children:** 一个长度为 26 的数组，存储对下一个字母节点的引用（假设只包含小写英文字母）。
2.  **isEnd:** 一个布尔值，表示该节点是否是某个字符串的结尾。

### 操作逻辑：
1.  **插入 (insert):** 遍历 `word` 的每个字符，如果当前字符对应的子节点不存在，则创建一个新的子节点。最后将末尾节点的 `isEnd` 置为 `true`。
2.  **查询 (search):** 遍历 `word`。如果中途某个字符对应的节点不存在，返回 `false`。遍历结束后，返回末尾节点的 `isEnd` 值。
3.  **前缀查询 (startsWith):** 逻辑同查询，但遍历结束后不需要判断 `isEnd`，只要路径存在即返回 `true`。

## 代码实现

```java
class Trie {
    private class TrieNode {
        private TrieNode[] children;
        private boolean isEnd;

        public TrieNode() {
            children = new TrieNode[26];
            isEnd = false;
        }
    }

    private TrieNode root;

    public Trie() {
        root = new TrieNode();
    }
    
    public void insert(String word) {
        TrieNode node = root;
        for (char c : word.toCharArray()) {
            int index = c - 'a';
            if (node.children[index] == null) {
                node.children[index] = new TrieNode();
            }
            node = node.children[index];
        }
        node.isEnd = true;
    }
    
    public boolean search(String word) {
        TrieNode node = searchPrefix(word);
        return node != null && node.isEnd;
    }
    
    public boolean startsWith(String prefix) {
        return searchPrefix(prefix) != null;
    }

    // 辅助方法：检索前缀是否存在，返回最后一个节点
    private TrieNode searchPrefix(String prefix) {
        TrieNode node = root;
        for (char c : prefix.toCharArray()) {
            int index = c - 'a';
            if (node.children[index] == null) {
                return null;
            }
            node = node.children[index];
        }
        return node;
    }
}
```

## 复杂度分析

*   **时间复杂度：**
    *   插入：$O(L)$，其中 $L$ 为字符串长度。
    *   查找：$O(L)$。
*   **空间复杂度：** $O(T \times 26)$，其中 $T$ 为所有插入字符串的字符总数。在最坏情况下（没有公共前缀），每个字符都会创建一个节点。
