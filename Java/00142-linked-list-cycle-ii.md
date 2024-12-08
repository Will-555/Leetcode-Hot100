[官方题解](https://leetcode.cn/problems/linked-list-cycle-ii/solutions/441131/huan-xing-lian-biao-ii-by-leetcode-solution/?envType=study-plan-v2&envId=top-100-liked)
```Java
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        int m = matrix.length - 1, n = matrix[0].length - 1;
        int left = n, top = 0;
        while (0 <= left && top <= m) {
            if (matrix[top][left] == target) return true;
            if (matrix[top][left] < target) top++;
            else left--;
public class Solution {
    public ListNode detectCycle(ListNode head) {
        if (head == null) return null;
        ListNode fast = head, slow = head;
        while (fast != null && fast.next != null) {
            fast = fast.next.next;
            slow = slow.next;
            if (fast == slow) break;
        }
        if (fast == null || fast.next == null) return null;
        slow = head;
        while (fast != slow) {
            slow = slow.next;
            fast = fast.next;
        }
        return false;
        return slow;
    }
}
```

```Java
public class Solution {
    public ListNode detectCycle(ListNode head) {
        if (head == null) {
            return null;
        }
        ListNode slow = head, fast = head;
        while (fast != null) {
            slow = slow.next;
            if (fast.next != null) {
                fast = fast.next.next;
            } else {
                return null;
            }
            if (fast == slow) {
                ListNode ptr = head;
                while (ptr != slow) {
                    ptr = ptr.next;
                    slow = slow.next;
                }
                return ptr;
            }
        }
        return null;
    }
}
```
