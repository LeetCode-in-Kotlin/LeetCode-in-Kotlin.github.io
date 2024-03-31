[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 295\. Find Median from Data Stream

Hard

The **median** is the middle value in an ordered integer list. If the size of the list is even, there is no middle value and the median is the mean of the two middle values.

*   For example, for `arr = [2,3,4]`, the median is `3`.
*   For example, for `arr = [2,3]`, the median is `(2 + 3) / 2 = 2.5`.

Implement the MedianFinder class:

*   `MedianFinder()` initializes the `MedianFinder` object.
*   `void addNum(int num)` adds the integer `num` from the data stream to the data structure.
*   `double findMedian()` returns the median of all elements so far. Answers within <code>10<sup>-5</sup></code> of the actual answer will be accepted.

**Example 1:**

**Input**

    ["MedianFinder", "addNum", "addNum", "findMedian", "addNum", "findMedian"]
    [[], [1], [2], [], [3], []]

**Output:** [null, null, null, 1.5, null, 2.0]

**Explanation:**

    MedianFinder medianFinder = new MedianFinder();
    medianFinder.addNum(1); // arr = [1]
    medianFinder.addNum(2); // arr = [1, 2]
    medianFinder.findMedian(); // return 1.5 (i.e., (1 + 2) / 2)
    medianFinder.addNum(3); // arr[1, 2, 3]
    medianFinder.findMedian(); // return 2.0 

**Constraints:**

*   <code>-10<sup>5</sup> <= num <= 10<sup>5</sup></code>
*   There will be at least one element in the data structure before calling `findMedian`.
*   At most <code>5 * 10<sup>4</sup></code> calls will be made to `addNum` and `findMedian`.

**Follow up:**

*   If all integer numbers from the stream are in the range `[0, 100]`, how would you optimize your solution?
*   If `99%` of all integer numbers from the stream are in the range `[0, 100]`, how would you optimize your solution?

## Solution

```kotlin
import java.util.PriorityQueue

class MedianFinder {
    // take two queues one is for storing upper half and the other is for lowerhalf
    // max stores the lower half
    // min heap stores the upper half
    private val maxHeap: PriorityQueue<Int> = PriorityQueue { a: Int, b: Int -> b - a }
    private val minHeap: PriorityQueue<Int> = PriorityQueue()
    fun addNum(num: Int) {
        if (maxHeap.isEmpty() || maxHeap.peek() > num) {
            maxHeap.offer(num)
        } else {
            minHeap.offer(num)
        }
        if (Math.abs(maxHeap.size - minHeap.size) > 1) {
            balance(maxHeap, minHeap)
        }
    }

    fun balance(maxHeap: PriorityQueue<Int>, minHeap: PriorityQueue<Int>) {
        val large = if (maxHeap.size > minHeap.size) maxHeap else minHeap
        val small = if (maxHeap.size > minHeap.size) minHeap else maxHeap
        small.offer(large.poll())
    }

    fun findMedian(): Double {
        val large = if (maxHeap.size > minHeap.size) maxHeap else minHeap
        val small = if (maxHeap.size > minHeap.size) minHeap else maxHeap
        return if (large.size == small.size) {
            (large.peek() + small.peek()).toDouble() / 2
        } else {
            large.peek().toDouble()
        }
    }
}

/*
 * Your MedianFinder object will be instantiated and called as such:
 * MedianFinder obj = new MedianFinder();
 * obj.addNum(num);
 * double param_2 = obj.findMedian();
 */
```