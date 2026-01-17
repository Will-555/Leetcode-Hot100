# 难度：中等

给你两个 **非空** 的链表，表示两个非负的整数。它们每位数字都是按照 **逆序** 的方式存储的，并且每个节点只能存储 **一位** 数字。

请你将两个数相加，并以相同形式返回一个表示和的链表。

你可以假设除了数字 0 之外，这两个数都不会以 0 开头。

**示例 1：**
输入：`l1 = [2,4,3], l2 = [5,6,4]`
输出：`[7,0,8]`
解释：`342 + 465 = 807`.

**示例 2：**
输入：`l1 = [0], l2 = [0]`
输出：`[0]`

**提示：**
- 每个链表中的节点数在范围 `[1, 100]` 内
- `0 <= Node.val <= 9`

```Java
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode prehead = new ListNode(0);
        ListNode curr = prehead;
        int carry = 0;
        while (l1 != null || l2 != null) {
            int x = (l1 != null) ? l1.val : 0;
            int y = (l2 != null) ? l2.val : 0;
            int sum = carry + x + y;
            carry = sum / 10;
            curr.next = new ListNode(sum % 10);
            curr = curr.next;
            if (l1 != null) l1 = l1.next;
            if (l2 != null) l2 = l2.next;
        }
        if (carry > 0) {
            curr.next = new ListNode(carry);
        }
        return prehead.next;
    }
}
```
