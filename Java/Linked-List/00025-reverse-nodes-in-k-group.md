# 难度：困难

给你一个链表，每 `k` 个节点一组进行翻转，请你返回翻转后的链表。

`k` 是一个正整数，它的值小于或等于链表的长度。

如果节点总数不是 `k` 的整数倍，那么请将最后剩余的节点保持原有顺序。

**示例 1：**
输入：`head = [1,2,3,4,5], k = 2`
输出：`[2,1,4,3,5]`

**示例 2：**
输入：`head = [1,2,3,4,5], k = 3`
输出：`[3,2,1,4,5]`

**提示：**
- 列表中节点的数量在范围 `[0, 5000]` 内
- `0 <= Node.val <= 1000`
- `1 <= k <= sz`

```Java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode reverseKGroup(ListNode head, int k) {
        ListNode dummy = new ListNode(0);
        dummy.next = head;

        ListNode pre = dummy;
        ListNode end = dummy;

        while (end.next != null) {
            // 1. 尝试找到当前组的最后一个节点
            for (int i = 0; i < k && end != null; i++) end = end.next;
            // 如果剩余节点不足 k 个，不翻转
            if (end == null) break;

            // 2. 标记当前组的开始节点和下一组的起始节点
            ListNode start = pre.next;
            ListNode next = end.next;

            // 3. 断开当前组与后面的连接以进行翻转
            end.next = null;
            pre.next = reverse(start);

            // 4. 将翻转后的部分与后面重新连接
            // 翻转后 start 变成了结尾
            start.next = next;

            // 5. 更新 pre 和 end 准备下一轮
            pre = start;
            end = pre;
        }

        return dummy.next;
    }

    // 标准单链表翻转
    private ListNode reverse(ListNode head) {
        ListNode pre = null;
        ListNode curr = head;
        while (curr != null) {
            ListNode next = curr.next;
            curr.next = pre;
            pre = curr;
            curr = next;
        }
        return pre;
    }
}
```

## 题解说明

### 核心思路

本题是“翻转链表”的进阶版本。我们需要将链表分成长度为 `k` 的小段，逐段翻转，并保持段间的正确连接。

### 1. 指针管理
使用四个关键节点来辅助翻转：
*   `pre`：当前待翻转组的前驱节点。
*   `start`：当前待翻转组的头节点。
*   `end`：当前待翻转组的尾节点。
*   `next`：下一组的头节点。

### 2. 执行步骤
1.  **探测长度**：从 `pre` 开始向后走 `k` 步找到 `end`。如果未走满 `k` 步即遇到 `null`，说明剩余节点不足一组，终止循环。
2.  **断开子链**：记录 `end.next` 为 `next`，并将 `end.next` 置为 `null`，形成一个独立的待翻转子链表。
3.  **局部翻转**：调用 `reverse(start)` 翻转该段。
4.  **接回原链**：
    *   将翻转后的新头（即原 `end`）接到 `pre.next`。
    *   将翻转后的新尾（即原 `start`）接到 `next`。
5.  **重置位置**：将 `pre` 和 `end` 都移动到 `start` 的位置，开始下一轮判定。

### 复杂度分析
*   **时间复杂度**：$O(n)$。每个节点最多被访问两次。
*   **空间复杂度**：$O(1)$。仅使用了常量级别的额外指针空间。
