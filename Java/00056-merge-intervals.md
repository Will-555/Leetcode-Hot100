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
