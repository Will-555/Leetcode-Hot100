```Java
class Solution {
    public ListNode mergeTwoLists(ListNode list1, ListNode list2) {
        ListNode head = new ListNode(0);
        ListNode dumpy = head;
        while (list1 != null && list2 != null) {
            if (list1.val < list2.val) {
                dumpy.next = new ListNode(list1.val);
                list1 = list1.next;
            } else {
                dumpy.next = new ListNode(list2.val);
                list2 = list2.next;
            }
            dumpy = dumpy.next;
        }
        dumpy.next = (list1 == null) ? list2 : list1;
        return head.next;
    }
}
```
