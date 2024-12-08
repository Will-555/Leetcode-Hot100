```Java
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode dumpy = new ListNode(0, head);
        ListNode slow = dumpy, fast = head;
        for (int i = 0; i < n; ++i) {
            fast = fast.next;
        }
        while (fast != null) {
            slow = slow.next;
            fast = fast.next;
        }   
        slow.next = slow.next.next;
        return dumpy.next;
    }
}
```
