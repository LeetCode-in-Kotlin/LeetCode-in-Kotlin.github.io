[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2163\. Minimum Difference in Sums After Removal of Elements

Hard

You are given a **0-indexed** integer array `nums` consisting of `3 * n` elements.

You are allowed to remove any **subsequence** of elements of size **exactly** `n` from `nums`. The remaining `2 * n` elements will be divided into two **equal** parts:

*   The first `n` elements belonging to the first part and their sum is <code>sum<sub>first</sub></code>.
*   The next `n` elements belonging to the second part and their sum is <code>sum<sub>second</sub></code>.

The **difference in sums** of the two parts is denoted as <code>sum<sub>first</sub> - sum<sub>second</sub></code>.

*   For example, if <code>sum<sub>first</sub> = 3</code> and <code>sum<sub>second</sub> = 2</code>, their difference is `1`.
*   Similarly, if <code>sum<sub>first</sub> = 2</code> and <code>sum<sub>second</sub> = 3</code>, their difference is `-1`.

Return _the **minimum difference** possible between the sums of the two parts after the removal of_ `n` _elements_.

**Example 1:**

**Input:** nums = [3,1,2]

**Output:** -1

**Explanation:** Here, nums has 3 elements, so n = 1.

Thus we have to remove 1 element from nums and divide the array into two equal parts.

- If we remove nums[0] = 3, the array will be [1,2]. The difference in sums of the two parts will be 1 - 2 = -1.

- If we remove nums[1] = 1, the array will be [3,2]. The difference in sums of the two parts will be 3 - 2 = 1.

- If we remove nums[2] = 2, the array will be [3,1]. The difference in sums of the two parts will be 3 - 1 = 2.

The minimum difference between sums of the two parts is min(-1,1,2) = -1. 

**Example 2:**

**Input:** nums = [7,9,5,8,1,3]

**Output:** 1

**Explanation:** Here n = 2. So we must remove 2 elements and divide the remaining array into two parts containing two elements each.

If we remove nums[2] = 5 and nums[3] = 8, the resultant array will be [7,9,1,3]. The difference in sums will be (7+9) - (1+3) = 12.

To obtain the minimum difference, we should remove nums[1] = 9 and nums[4] = 1. The resultant array becomes [7,5,8,3]. The difference in sums of the two parts is (7+5) - (8+3) = 1.

It can be shown that it is not possible to obtain a difference smaller than 1. 

**Constraints:**

*   `nums.length == 3 * n`
*   <code>1 <= n <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>5</sup></code>

## Solution

```kotlin
import java.util.PriorityQueue

class Solution {
    fun minimumDifference(nums: IntArray): Long {
        val n = nums.size / 3
        val minHeap = PriorityQueue<Int>()
        val maxHeap = PriorityQueue { a: Int, b: Int -> b - a }
        val leftMemo = LongArray(nums.size)
        val rightMemo = LongArray(nums.size)
        var current = 0L
        for (i in 0..2 * n - 1) {
            current += nums[i].toLong()
            maxHeap.add(nums[i])
            if (maxHeap.size > n) {
                val removed = maxHeap.poll()
                current -= removed.toLong()
                leftMemo[i] = current
            }
            if (maxHeap.size == n) {
                leftMemo[i] = current
            }
        }
        current = 0
        for (i in nums.size - 1 downTo n) {
            current += nums[i].toLong()
            minHeap.add(nums[i])
            if (minHeap.size > n) {
                val removed = minHeap.poll()
                current -= removed.toLong()
            }
            if (minHeap.size == n) {
                rightMemo[i] = current
            }
        }
        var min = Long.MAX_VALUE
        for (i in n - 1..2 * n - 1) {
            min = Math.min(min, leftMemo[i] - rightMemo[i + 1])
        }
        return min
    }
}
```