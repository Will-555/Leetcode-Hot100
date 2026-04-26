# 148. 排序链表 (Sort List)

[LeetCode 题目链接](https://leetcode.cn/problems/sort-list/)

## 题目描述

给你链表的头结点 `head` ，请将其按 **升序** 排列并返回 **排序后的链表** 。

**进阶：** 你可以在 $O(n \log n)$ 时间复杂度和常数级空间复杂度（不考虑递归栈）下解决这个问题吗？

## 解题思路

链表的排序首选 **归并排序 (Merge Sort)**，因为链表通过改变指针即可修改结构，不需要像数组那样开辟额外的 O(n) 空间。

### 归并排序步骤：
1.  **切分 (Split)：** 利用快慢指针找到链表的中点，将链表切断，分成两个子链表。
2.  **递归 (Recursion)：** 对两个子链表分别递归调用排序函数。
3.  **合并 (Merge)：** 将两个有序链表合并为一个有序链表（类似 LeetCode 21 题）。

## 代码实现

```java
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
    public ListNode sortList(ListNode head) {
        // 递归终止条件
        if (head == null || head.next == null) {
            return head;
        }

        // 1. 利用快慢指针找到中点并切断
        ListNode slow = head;
        ListNode fast = head.next; // 注意这里 fast 从 head.next 开始，方便切分
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }
        
        ListNode secondHalf = slow.next;
        slow.next = null; // 断开链表

        // 2. 递归排序左右两半
        ListNode left = sortList(head);
        ListNode right = sortList(secondHalf);

        // 3. 合并有序链表
        return merge(left, right);
    }

    private ListNode merge(ListNode l1, ListNode l2) {
        ListNode dummy = new ListNode(0);
        ListNode current = dummy;
        
        while (l1 != null && l2 != null) {
            if (l1.val < l2.val) {
                current.next = l1;
                l1 = l1.next;
            } else {
                current.next = l2;
                l2 = l2.next;
            }
            current = current.next;
        }
        
        if (l1 != null) current.next = l1;
        if (l2 != null) current.next = l2;
        
        return dummy.next;
    }
}
```

## 复杂度分析

*   **时间复杂度：** $O(n \log n)$，其中 $n$ 是链表的长度。
*   **空间复杂度：** $O(\log n)$，主要是递归调用的栈空间。如果要做到 $O(1)$ 空间，则需要使用迭代版的归并排序。
