# 难度：中等

## 题目描述
给定一个包含 **随机指针** 的链表，每个节点除了有一个 `next` 指针外，还有一个 `random` 指针指向链表中的任意节点或 `null`。请你返回该链表的 **深拷贝**（复制后每个节点的 `next` 与 `random` 指针都指向新链表中的对应节点），并且不改变原链表。

**示例**
```
输入: head = [[7,null],[13,0],[11,4],[10,2],[1,0]]
输出: [[7,null],[13,0],[11,4],[10,2],[1,0]]
解释: 节点 0 的值是 7，random 指向 null。
      节点 1 的值是 13，random 指向节点 0（值为 7）。
      节点 2 的值是 11，random 指向节点 4（值为 1）。
      节点 3 的值是 10，random 指向节点 2（值为 11）。
      节点 4 的值是 1，random 指向节点 0（值为 7）。
```

**提示**
- `0 <= 节点数 <= 1000`
- `-10000 <= Node.val <= 10000`
- `Node.random` 可以指向链表中的任意节点或 `null`

## 解题思路（哈希表 + O(1) 空间）
1. **哈希表法**：遍历原链表，为每个原节点创建对应的新节点并存入 `Map<Node, Node>`，第二遍遍历时利用映射设置 `next` 与 `random`。时间 `O(n)`，空间 `O(n)`。
2. **O(1) 空间法**（官方推荐）：
   - 第一次遍历，在每个原节点后插入其拷贝节点（`old.next = copy; copy.next = nextOld`），形成交错链表。
   - 第二次遍历，利用 `copy.random = old.random != null ? old.random.next : null` 完成 `random` 指针的复制。
   - 第三次遍历，将交错链表拆分为原链表和拷贝链表。
   该方法只使用常数额外空间，仍然是 `O(n)` 时间。

## 代码实现（Java）
```java
import java.util.*;

/**
 * Definition for a Node.
 */
class Node {
    int val;
    Node next;
    Node random;
    Node(int val) { this.val = val; }
}

class Solution {
    /**
     * 返回链表的深拷贝，使用 O(1) 额外空间的交错链表技巧。
     */
    public Node copyRandomList(Node head) {
        if (head == null) return null;
        // 1. 在每个原节点后插入复制节点
        for (Node cur = head; cur != null; ) {
            Node copy = new Node(cur.val);
            copy.next = cur.next;
            cur.next = copy;
            cur = copy.next;
        }
        // 2. 设置复制节点的 random 指针
        for (Node cur = head; cur != null; cur = cur.next.next) {
            if (cur.random != null) {
                cur.next.random = cur.random.next; // copy.random = original.random.next
            }
        }
        // 3. 拆分交错链表，恢复原链表并抽取复制链表
        Node dummy = new Node(0);
        Node copyPrev = dummy;
        for (Node cur = head; cur != null; ) {
            Node copy = cur.next;
            // 恢复原链表的 next
            cur.next = copy.next;
            // 将复制节点接入新链表
            copyPrev.next = copy;
            copyPrev = copy;
            // 前进到下一个原节点
            cur = cur.next;
        }
        return dummy.next;
    }
}
```

## 复杂度分析
- **时间复杂度**：`O(n)`，遍历链表三次。
- **空间复杂度**：`O(1)`（不计返回的复制链表），只使用了常数个额外指针。
