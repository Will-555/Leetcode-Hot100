```Java
class Solution {
    public ListNode reverseList(ListNode head) {
        ListNode dumpy = null;
        while (head != null) {
            ListNode node = new ListNode(head.val);
            node.next = dumpy;
            dumpy = node;
            head = head.next;
        }
        return dumpy;
    }
}
```
