# 难度：中等

给定一个整数数组 `nums` 和一个整数 `k`，请返回数组中第 `k` 个最大的元素。

请注意，你需要找的是数组排序后的第 `k` 个最大的元素，而不是第 `k` 个不同的元素。

你必须设计并实现时间复杂度为 `O(n)` 的算法解决此问题。

**示例 1:**
输入: `[3,2,1,5,6,4], k = 2`
输出: `5`

**示例 2:**
输入: `[3,2,3,1,2,4,5,5,6], k = 4`
输出: `4`

**提示：**
- `1 <= k <= nums.length <= 10^5`
- `-10^4 <= nums[i] <= 10^4`

```Java
class Solution {
    /**
     * 解法 1：堆排序（优先队列）
     * 时间复杂度：O(n log k)，空间复杂度：O(k)
     */
    public int findKthLargest(int[] nums, int k) {
        // 维持一个大小为 k 的小顶堆
        PriorityQueue<Integer> heap = new PriorityQueue<>();
        for (int num : nums) {
            heap.add(num);
            // 如果堆的大小超过 k，弹出堆顶（最小的）
            // 剩余的 k 个元素就是数组中最大的 k 个数，其中堆顶是这 k 个数里最小的，即第 k 大
            if (heap.size() > k) {
                heap.poll();
            }
        }
        return heap.peek();
    }

    /**
     * 解法 2：快速选择 (Quickselect)
     * 时间复杂度：平均 O(n)，最坏 O(n^2)，空间复杂度：O(log n)
     * 这是满足题目要求的 O(n) 时间复杂度的标准做法（平均意义下）
     */
    public int findKthLargest_QuickSelect(int[] nums, int k) {
        int n = nums.length;
        // 第 k 大对应于排序后的第 n - k 个位置（从小到大索引）
        return quickselect(nums, 0, n - 1, n - k);
    }

    private int quickselect(int[] nums, int l, int r, int target) {
        if (l == r) return nums[l];
        
        // 随机选择基准值以避免极端情况下的 O(n^2) 复杂度
        int pivotIndex = l + new Random().nextInt(r - l + 1);
        int partitionIndex = partition(nums, l, r, pivotIndex);
        
        if (partitionIndex == target) {
            return nums[partitionIndex];
        } else if (partitionIndex < target) {
            // 在右侧寻找
            return quickselect(nums, partitionIndex + 1, r, target);
        } else {
            // 在左侧寻找
            return quickselect(nums, l, partitionIndex - 1, target);
        }
    }

    private int partition(int[] nums, int l, int r, int pivotIndex) {
        int pivot = nums[pivotIndex];
        swap(nums, pivotIndex, r); // 把基准移到最后
        int i = l;
        for (int j = l; j < r; j++) {
            if (nums[j] < pivot) {
                swap(nums, i, j);
                i++;
            }
        }
        swap(nums, i, r); // 把基准移到它最终的位置 i
        return i;
    }

    /**
     * 解法 3：手动建立大顶堆
     * 时间复杂度：O(n + k log n)，空间复杂度：O(log n)（递归调用栈）
     */
    public int findKthLargest_MaxHeap(int[] nums, int k) {
        int heapSize = nums.length;
        // 1. 建立初始大顶堆
        buildMaxHeap(nums, heapSize);
        // 2. 进行 k-1 次删除操作（将堆顶即当前最大值交换到末尾，并重新调整堆）
        for (int i = nums.length - 1; i >= nums.length - k + 1; --i) {
            swap(nums, 0, i);
            --heapSize;
            maxHeapify(nums, 0, heapSize);
        }
        // 3. 此时堆顶元素就是第 k 个最大的元素
        return nums[0];
    }

    public void buildMaxHeap(int[] a, int heapSize) {
        // 从最后一个非叶子节点开始向上调整
        for (int i = heapSize / 2 - 1; i >= 0; --i) {
            maxHeapify(a, i, heapSize);
        } 
    }

    public void maxHeapify(int[] a, int i, int heapSize) {
        int l = i * 2 + 1, r = i * 2 + 2, largest = i;
        if (l < heapSize && a[l] > a[largest]) {
            largest = l;
        } 
        if (r < heapSize && a[r] > a[largest]) {
            largest = r;
        }
        if (largest != i) {
            swap(a, i, largest);
            // 递归调整被交换的子树
            maxHeapify(a, largest, heapSize);
        }
    }

    private void swap(int[] nums, int i, int j) {
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }
}
```

## 题解说明

### 1. 堆排序法（优先队列）
*   **原理**：利用小顶堆维护数组中最大的 $k$ 个元素。
*   **复杂度**：$O(n \log k)$。适合海量数据流。

### 2. 快速选择法 (Quickselect)
*   **原理**：基于快速排序思想。平均时间复杂度为 $O(n)$，是理论上解决此题最快的方法。

### 3. 手动大顶堆法
*   **原理**：先将整个数组建立为一个大顶堆。大顶堆的堆顶是当前数组的最大值。我们只需要执行 $k-1$ 次“删除堆顶”操作（即将堆顶与当前堆的最后一个元素交换，然后重新调整），第 $k$ 次留在堆顶的元素就是目标值。
*   **复杂度分析**：
    *   建堆的时间复杂度为 $O(n)$。
    *   每次调整堆的时间复杂度为 $O(\log n)$，执行 $k$ 次，总计 $O(k \log n)$。
    *   总时间复杂度为 $O(n + k \log n)$。

### 总结
*   **Quickselect** 理论最优。
*   **小顶堆（PriorityQueue）** 在处理流式数据时最节省空间。
*   **大顶堆（手动实现）** 展示了底层堆排序的完整过程，常用于考查堆的底层原理。
