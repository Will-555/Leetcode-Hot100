[官方题解](https://leetcode.cn/problems/merge-k-sorted-lists/solutions/219756/he-bing-kge-pai-xu-lian-biao-by-leetcode-solutio-2/?envType=study-plan-v2&envId=top-100-liked)  
```Java
class Solution {
    public ListNode mergeKLists(ListNode[] lists) {
        return merge(lists, 0, lists.length - 1);
    }

    private ListNode merge(ListNode[] lists, int l, int r) {
        if (l == r) return lists[l];
        if (l > r) return null;
        int mid = l + (r - l) / 2;
        return mergeTwoList(merge(lists, l, mid), merge(lists, mid + 1, r));
    }

    private ListNode mergeTwoList(ListNode a, ListNode b) {
        if (a == null || b == null) return a == null ? b : a;
        ListNode head = new ListNode(0);
        ListNode dumpy = head;
        while (a != null && b != null) {
            if (a.val < b.val) {
                dumpy.next = a; a = a.next;
            } else {
                dumpy.next = b; b = b.next;
            }
            dumpy = dumpy.next;
        }
        dumpy.next = a == null ? b : a;
        return head.next;
    }
}
```
