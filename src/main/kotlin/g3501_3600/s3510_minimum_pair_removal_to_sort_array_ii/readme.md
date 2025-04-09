[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3510\. Minimum Pair Removal to Sort Array II

Hard

Given an array `nums`, you can perform the following operation any number of times:

*   Select the **adjacent** pair with the **minimum** sum in `nums`. If multiple such pairs exist, choose the leftmost one.
*   Replace the pair with their sum.

Return the **minimum number of operations** needed to make the array **non-decreasing**.

An array is said to be **non-decreasing** if each element is greater than or equal to its previous element (if it exists).

**Example 1:**

**Input:** nums = [5,2,3,1]

**Output:** 2

**Explanation:**

*   The pair `(3,1)` has the minimum sum of 4. After replacement, `nums = [5,2,4]`.
*   The pair `(2,4)` has the minimum sum of 6. After replacement, `nums = [5,6]`.

The array `nums` became non-decreasing in two operations.

**Example 2:**

**Input:** nums = [1,2,2]

**Output:** 0

**Explanation:**

The array `nums` is already sorted.

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>-10<sup>9</sup> <= nums[i] <= 10<sup>9</sup></code>

## Solution

```kotlin
class Solution {
    private class Segment {
        private val start: Int
        private val end: Int
        private var left: Segment? = null
        private var right: Segment? = null
        private var lIdx: Int = 0
        private var lNum: Long = 0
        private var rIdx: Int = 0
        private var rNum: Long = 0
        var ok: Boolean = false
        var minSum: Long = 0
        var li: Int = 0
        var ri: Int = 0

        companion object {
            fun init(arr: IntArray): Segment {
                return Segment(arr, 0, arr.size - 1)
            }
        }

        constructor(arr: IntArray, s: Int, e: Int) {
            start = s
            end = e
            if (s >= e) {
                lIdx = s
                rIdx = s
                lNum = arr[s].toLong()
                rNum = arr[s].toLong()
                minSum = Long.MAX_VALUE
                ok = true
                return
            }
            val mid = s + ((e - s) shr 1)
            left = Segment(arr, s, mid)
            right = Segment(arr, mid + 1, e)
            merge()
        }

        private fun merge() {
            left?.let { left ->
                right?.let { right ->
                    lIdx = left.lIdx
                    lNum = left.lNum
                    rIdx = right.rIdx
                    rNum = right.rNum
                    ok = left.ok && right.ok && left.rNum <= right.lNum
                    minSum = left.minSum
                    li = left.li
                    ri = left.ri
                    if (left.rNum + right.lNum < minSum) {
                        minSum = left.rNum + right.lNum
                        li = left.rIdx
                        ri = right.lIdx
                    }
                    if (right.minSum < minSum) {
                        minSum = right.minSum
                        li = right.li
                        ri = right.ri
                    }
                }
            }
        }

        fun update(i: Int, n: Long) {
            if (start <= i && end >= i) {
                if (start >= end) {
                    lNum = n
                    rNum = n
                } else {
                    left?.update(i, n)
                    right?.update(i, n)
                    merge()
                }
            }
        }

        fun remove(i: Int): Segment? {
            if (start > i || end < i) {
                return this
            } else if (start >= end) {
                return null
            }
            left = left?.remove(i)
            right = right?.remove(i)
            if (left == null) {
                return right
            } else if (right == null) {
                return left
            }
            merge()
            return this
        }
    }

    fun minimumPairRemoval(nums: IntArray): Int {
        var root = Segment.init(nums)
        var res = 0
        while (!root.ok) {
            val l = root.li
            val r = root.ri
            root.update(l, root.minSum)
            root = root.remove(r) ?: break
            res++
        }
        return res
    }
}
```