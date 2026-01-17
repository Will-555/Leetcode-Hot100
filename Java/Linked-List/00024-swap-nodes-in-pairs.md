```Java
class Solution {
    public ListNode swapPairs(ListNode head) {
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        ListNode cur = dummy;
        while (cur != null && cur.next != null && cur.next.next != null) {
            ListNode next = new ListNode(cur.next.val);
            ListNode next2 = new ListNode(cur.next.next.val);
            next.next = cur.next.next.next;
            cur.next = next2;
            cur.next.next = next;
            cur = cur.next.next;
        }
        return dummy.next;
    }
}
```
