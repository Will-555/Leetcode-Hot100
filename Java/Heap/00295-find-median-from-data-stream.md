# 难度：困难

**中位数**是有序整数列表中的中间值。如果列表的大小是偶数，则没有中间值，中位数是两个中间值的平均值。

例如：
- 对于 `arr = [2,3,4]`，中位数是 `3`。
- 对于 `arr = [2,3]`，中位数是 `(2 + 3) / 2 = 2.5`。

实现 `MedianFinder` 类:
- `MedianFinder()` 初始化 `MedianFinder` 对象。
- `void addNum(int num)` 将数据流中的整数 `num` 添加到数据结构中。
- `double findMedian()` 返回到目前为止所有元素的中位数。与实际答案相差 `10^-5` 以内的答案将被接受。

**示例 1：**
输入：
`["MedianFinder", "addNum", "addNum", "findMedian", "addNum", "findMedian"]`
`[[], [1], [2], [], [3], []]`
输出：
`[null, null, null, 1.5, null, 2.0]`

解释：
```Java
MedianFinder medianFinder = new MedianFinder();
medianFinder.addNum(1);    // arr = [1]
medianFinder.addNum(2);    // arr = [1, 2]
medianFinder.findMedian(); // 返回 1.5 (即，(1 + 2) / 2)
medianFinder.addNum(3);    // arr[1, 2, 3]
medianFinder.findMedian(); // 返回 2.0
```

**提示:**
- `-10^5 <= num <= 10^5`
- 在调用 `findMedian` 之前，数据结构中至少有一个元素
- 最多 `5 * 10^4` 次调用 `addNum` 和 `findMedian`

```Java
class MedianFinder {
    // 小顶堆，存储较大的一半元素
    PriorityQueue<Integer> minHeap;
    // 大顶堆，存储较小的一半元素
    PriorityQueue<Integer> maxHeap;

    public MedianFinder() {
        // 小顶堆（右半部分）：堆顶是最小的
        minHeap = new PriorityQueue<>((a, b) -> a - b);
        // 大顶堆（左半部分）：堆顶是最大的
        maxHeap = new PriorityQueue<>((a, b) -> b - a);
    }
    
    public void addNum(int num) {
        // 决策添加在哪个堆：
        // 如果 maxHeap 为空，或者新数小于等于左侧最大值，放入左侧大顶堆
        if (maxHeap.isEmpty() || num <= maxHeap.peek()) {
            maxHeap.offer(num);
        } else {
            minHeap.offer(num);
        }
        
        // 动态平衡：保证 maxHeap 的大小等于 minHeap 或者比它多 1
        if (maxHeap.size() > minHeap.size() + 1) {
            minHeap.offer(maxHeap.poll());
        } else if (minHeap.size() > maxHeap.size()) {
            maxHeap.offer(minHeap.poll());
        }
    }
    
    public double findMedian() {
        // 如果总数是奇数，maxHeap 多出一个，中位数就在 maxHeap 堆顶
        if (maxHeap.size() > minHeap.size()) {
            return maxHeap.peek();
        }
        // 如果是偶数，取两个堆顶的平均值
        return (maxHeap.peek() + minHeap.peek()) / 2.0;
    }
}
```

## 题解说明

### 核心思路：双堆法 (Two Heaps)
为了高效找到中位数，我们不需要对整个数据流进行实时全排序。关键在于将数据分成**两半**：
1.  **左半部分 (maxHeap)**：是一个大顶堆，存放数据流中较小的那些数，堆顶是左半部分的最大值。
2.  **右半部分 (minHeap)**：是一个小顶堆，存放数据流中较大的那些数，堆顶是右半部分的最小值。

### 1. 为什么用堆？
*   中位数只取决于中间的一两个数。
*   左侧的最大值和右侧的最小值正好是中位数的候选者。
*   堆这种数据结构可以 $O(1)$ 时间获取极值，$O(\log n)$ 平衡数据。

### 2. 执行流程
*   `addNum`：新数字根据其大小进入对应的堆，然后通过 `poll` 和 `offer` 来维持两边的大小平衡。
*   `findMedian`：根据总数的奇偶性，从一个堆顶或两个堆顶中计算结果。

### 复杂度分析
*   **时间复杂度**：
    *   `addNum`：$O(\log n)$。
    *   `findMedian`：$O(1)$。
*   **空间复杂度**：$O(n)$。存储了全部数据。
