# 难度：中等

你这个学期必须选修 `numCourses` 门课程，记为 `0` 到 `numCourses - 1` 。

在选修某些课程之前需要一些先修课程。 先修课程按数组 `prerequisites` 给出，其中 `prerequisites[i] = [ai, bi]` ，表示如果要学习课程 `ai` 则 **必须** 先学习课程  `bi` 。

例如，先修课程对 `[0, 1]` 表示：想要学习课程 `0` ，你需要先完成课程 `1` 。
请你判断是否可能完成所有课程的学习？如果可以，返回 `true` ；否则，返回 `false` 。

**示例 1：**
输入：`numCourses = 2, prerequisites = [[1,0]]`
输出：`true`
解释：总共有 2 门课程。学习课程 1 之前，你需要完成课程 0 。这是可能的。

**示例 2：**
输入：`numCourses = 2, prerequisites = [[1,0],[0,1]]`
输出：`false`
解释：总共有 2 门课程。学习课程 1 之前，你需要先完成​课程 0 ；并且学习课程 0 之前，你还应先完成课程 1 。这显然是不可能的。

**提示：**
- `1 <= numCourses <= 2000`
- `0 <= prerequisites.length <= 5000`
- `prerequisites[i].length == 2`
- `0 <= ai, bi < numCourses`
- `prerequisites[i]` 中的所有课程对 **互不相同**

```Java
```java
class Solution {
    /**
     * 判断是否可以完成所有课程的学习（拓扑排序）
     * @param numCourses 课程总数，编号为 0 .. numCourses-1
     * @param prerequisites 前置课程数组，每个元素 [a, b] 表示学习 a 前必须先学 b
     * @return 若不存在环且所有课程都能被选修返回 true，否则 false
     */
    public boolean canFinish(int numCourses, int[][] prerequisites) {
        // 入度数组，记录每门课程需要的先修课程数量
        int[] indegrees = new int[numCourses];
        // 邻接表，记录每门课程作为先修课时可以解锁的后续课程列表
        List<List<Integer>> adjacency = new ArrayList<>();
        for (int i = 0; i < numCourses; i++) {
            adjacency.add(new ArrayList<>());
        }
        // 构建图：统计入度并填充邻接表
        for (int[] cp : prerequisites) {
            int course = cp[0];   // 目标课程
            int pre = cp[1];      // 先修课程
            indegrees[course]++;               // 目标课程的入度 +1
            adjacency.get(pre).add(course);    // 先修课程指向目标课程
        }
        // 将所有入度为 0（即没有先修要求）的课程加入队列，作为拓扑排序的起点
        Queue<Integer> queue = new LinkedList<>();
        for (int i = 0; i < numCourses; i++) {
            if (indegrees[i] == 0) {
                queue.offer(i);
            }
        }
        // BFS 进行拓扑排序，逐步移除已选修课程并更新其后继课程的入度
        while (!queue.isEmpty()) {
            int pre = queue.poll();   // 选修当前先修课程
            numCourses--;            // 剩余未选修课程数减一
            for (int cur : adjacency.get(pre)) {
                // 对每个受该先修课程影响的后续课程，入度减一
                if (--indegrees[cur] == 0) {
                    // 入度降至 0，说明所有先修要求已满足，可加入队列继续选修
                    queue.offer(cur);
                }
            }
        }
        // 若所有课程都被选修（numCourses 归零），说明不存在环，返回 true
        return numCourses == 0;
    }
}
```
```
