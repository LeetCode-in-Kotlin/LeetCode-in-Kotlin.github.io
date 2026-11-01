[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3801\. Minimum Cost to Merge Sorted Lists

Hard

You are given a 2D integer array `lists`, where each `lists[i]` is a non-empty array of integers **sorted** in **non-decreasing** order.

You may **repeatedly** choose two lists `a = lists[i]` and `b = lists[j]`, where `i != j`, and merge them. The **cost** to merge `a` and `b` is:

`len(a) + len(b) + abs(median(a) - median(b))`, where `len` and `median` denote the list length and median, respectively.

After merging `a` and `b`, remove both `a` and `b` from `lists` and insert the new merged **sorted list** in **any** position. Repeat merges until only **one** list remains.

Return an integer denoting the **minimum total cost** required to merge all lists into one single sorted list.

The **median** of an array is the middle element after sorting it in non-decreasing order. If the array has an even number of elements, the median is the left middle element.

**Example 1:**

**Input:** lists = \[\[1,3,5],[2,4],[6,7,8]]

**Output:** 18

**Explanation:**

Merge `a = [1, 3, 5]` and `b = [2, 4]`:

*   `len(a) = 3` and `len(b) = 2`
*   `median(a) = 3` and `median(b) = 2`
*   `cost = len(a) + len(b) + abs(median(a) - median(b)) = 3 + 2 + abs(3 - 2) = 6`

So `lists` becomes `[[1, 2, 3, 4, 5], [6, 7, 8]]`.

Merge `a = [1, 2, 3, 4, 5]` and `b = [6, 7, 8]`:

*   `len(a) = 5` and `len(b) = 3`
*   `median(a) = 3` and `median(b) = 7`
*   `cost = len(a) + len(b) + abs(median(a) - median(b)) = 5 + 3 + abs(3 - 7) = 12`

So `lists` becomes `[[1, 2, 3, 4, 5, 6, 7, 8]]`, and total cost is `6 + 12 = 18`.

**Example 2:**

**Input:** lists = \[\[1,1,5],[1,4,7,8]]

**Output:** 10

**Explanation:**

Merge `a = [1, 1, 5]` and `b = [1, 4, 7, 8]`:

*   `len(a) = 3` and `len(b) = 4`
*   `median(a) = 1` and `median(b) = 4`
*   `cost = len(a) + len(b) + abs(median(a) - median(b)) = 3 + 4 + abs(1 - 4) = 10`

So `lists` becomes `[[1, 1, 1, 4, 5, 7, 8]]`, and total cost is 10.

**Example 3:**

**Input:** lists = \[\[1],[3]]

**Output:** 4

**Explanation:**

Merge `a = [1]` and `b = [3]`:

*   `len(a) = 1` and `len(b) = 1`
*   `median(a) = 1` and `median(b) = 3`
*   `cost = len(a) + len(b) + abs(median(a) - median(b)) = 1 + 1 + abs(1 - 3) = 4`

So `lists` becomes `[[1, 3]]`, and total cost is 4.

**Example 4:**

**Input:** lists = \[\[1],[1]]

**Output:** 2

**Explanation:**

The total cost is `len(a) + len(b) + abs(median(a) - median(b)) = 1 + 1 + abs(1 - 1) = 2`.

**Constraints:**

*   `2 <= lists.length <= 12`
*   `1 <= lists[i].length <= 500`
*   <code>-10<sup>9</sup> <= lists[i][j] <= 10<sup>9</sup></code>
*   `lists[i]` is sorted in non-decreasing order.
*   The **sum** of `lists[i].length` will not exceed 2000.

## Solution

```kotlin
import kotlin.math.abs
import kotlin.math.min

class Solution {
    private fun numLt(lists: Array<IntArray>, enabled: Int, guess: Int): Int {
        var result = 0
        for (i in lists.indices) {
            if (((enabled shr i) and 1) == 1) {
                val list = lists[i]
                var low = 0
                var high = list.size
                while (low < high) {
                    val mid = (low + high) / 2
                    if (list[mid] < guess) {
                        low = mid + 1
                    } else {
                        high = mid
                    }
                }
                result += low
            }
        }
        return result
    }

    fun minMergeCost(lists: Array<IntArray>): Long {
        val n = lists.size
        val allNums = getSortedNumbers(lists)
        val subsets = buildSubsetInfo(lists, allNums)
        val dp = LongArray(1 shl n)
        dp.fill(Long.MAX_VALUE)
        for (subset in 0..<(1 shl n)) {
            if (Integer.bitCount(subset) <= 1) {
                dp[subset] = 0
                continue
            }
            dp[subset] = calculateMinCost(subset, subsets, dp)
        }
        return dp[(1 shl n) - 1]
    }

    private fun getSortedNumbers(lists: Array<IntArray>): MutableList<Int> {
        val allNums: MutableList<Int> = ArrayList<Int>()
        for (lst in lists) {
            for (num in lst) {
                allNums.add(num)
            }
        }
        allNums.sort()
        return allNums
    }

    private fun buildSubsetInfo(lists: Array<IntArray>, allNums: MutableList<Int>): Array<LongArray?> {
        val n = lists.size
        val subsets = Array<LongArray?>(1 shl n) { LongArray(2) }
        for (subset in 1..<(1 shl n)) {
            val resultLen = getSubsetLength(lists, subset)
            val medianLt = (resultLen - 1) / 2
            subsets[subset]!![0] = resultLen.toLong()
            subsets[subset]!![1] = findMedian(lists, subset, medianLt, allNums).toLong()
        }
        return subsets
    }

    private fun getSubsetLength(lists: Array<IntArray>, subset: Int): Int {
        var resultLen = 0
        for (i in lists.indices) {
            if (((subset shr i) and 1) == 1) {
                resultLen += lists[i].size
            }
        }
        return resultLen
    }

    private fun findMedian(lists: Array<IntArray>, subset: Int, medianLt: Int, allNums: MutableList<Int>): Int {
        var low = 0
        var high = allNums.size - 1
        var actualMedian = -1
        while (low <= high) {
            val mid = (low + high) / 2
            val num: Int = allNums[mid]
            if (numLt(lists, subset, num) <= medianLt) {
                actualMedian = num
                low = mid + 1
            } else {
                high = mid - 1
            }
        }
        return actualMedian
    }

    private fun calculateMinCost(subset: Int, subsets: Array<LongArray?>, dp: LongArray): Long {
        var minCost = Long.MAX_VALUE
        var a = (subset - 1) and subset
        while (a > 0) {
            val b = subset xor a
            val aLen = subsets[a]!![0]
            val aMedian = subsets[a]!![1]
            val bLen = subsets[b]!![0]
            val bMedian = subsets[b]!![1]
            val cost = aLen + bLen + abs(aMedian - bMedian)
            minCost = min(minCost, dp[a] + dp[b] + cost)
            a = (a - 1) and subset
        }
        return minCost
    }
}
```