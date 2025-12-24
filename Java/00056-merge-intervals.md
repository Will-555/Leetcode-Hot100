## 56. 合并区间
以数组 intervals 表示若干个区间的集合，其中单个区间为 intervals[i] = [starti, endi] 。请你合并所有重叠的区间，并返回 一个不重叠的区间数组，该数组需恰好覆盖输入中的所有区间 。


示例 1：
输入：intervals = [[1,3],[2,6],[8,10],[15,18]]
输出：[[1,6],[8,10],[15,18]]
解释：区间 [1,3] 和 [2,6] 重叠, 将它们合并为 [1,6].

示例 2：
输入：intervals = [[1,4],[4,5]]
输出：[[1,5]]
解释：区间 [1,4] 和 [4,5] 可被视为重叠区间。

示例 3：
输入：intervals = [[4,7],[1,4]]
输出：[[1,7]]
解释：区间 [1,4] 和 [4,7] 可被视为重叠区间。
 

提示：
1 <= intervals.length <= 104
intervals[i].length == 2
0 <= starti <= endi <= 104

```Java
class Solution {
    public int[][] merge(int[][] intervals) {
        int len = intervals.length;
        if (len < 2) return intervals;
        Arrays.sort(intervals, new Comparator<int[]>() {
            public int compare(int[] a, int[] b) {
                return a[0] == b[0] ? a[1] - b[1] : a[0] - b[0]; 
            }
        });
        int[] temp = intervals[0];
        List<int[]> ans = new ArrayList<>();
        for (int i = 1; i < len; ++i) {
            if (intervals[i][0] <= temp[1]) temp[1] = Math.max(temp[1], intervals[i][1]);
            else {
                ans.add(temp);
                temp = intervals[i];
            }
        }
        ans.add(temp);
        return ans.toArray(new int[ans.size()][]);
    }
}
```
