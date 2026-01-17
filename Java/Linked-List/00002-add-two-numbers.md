```Java
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode ans = new ListNode(0), dumpy = ans;
        int carry = 0;
        while (l1 != null && l2 != null) {
            int sum = l1.val + l2.val + carry;
            dumpy.next = new ListNode(sum % 10);
            carry = sum / 10;
            dumpy = dumpy.next; l1 = l1.next; l2 = l2.next;
        }
        ListNode node = l1 == null ? l2 : l1;
        while (node != null) {
            int sum = node.val + carry;
            carry = sum / 10;
            dumpy.next = new ListNode(sum % 10);
            dumpy = dumpy.next; node = node.next;
        }
        if (0 < carry) dumpy.next = new ListNode(carry);
        return ans.next;
    }
}
```
