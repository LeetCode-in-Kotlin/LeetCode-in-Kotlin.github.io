[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3501\. Maximize Active Section with Trade II

Hard

You are given a binary string `s` of length `n`, where:

*   `'1'` represents an **active** section.
*   `'0'` represents an **inactive** section.

You can perform **at most one trade** to maximize the number of active sections in `s`. In a trade, you:

*   Convert a contiguous block of `'1'`s that is surrounded by `'0'`s to all `'0'`s.
*   Afterward, convert a contiguous block of `'0'`s that is surrounded by `'1'`s to all `'1'`s.

Additionally, you are given a **2D array** `queries`, where <code>queries[i] = [l<sub>i</sub>, r<sub>i</sub>]</code> represents a **substring** <code>s[l<sub>i</sub>...r<sub>i</sub>]</code>.

For each query, determine the **maximum** possible number of active sections in `s` after making the optimal trade on the substring <code>s[l<sub>i</sub>...r<sub>i</sub>]</code>.

Return an array `answer`, where `answer[i]` is the result for `queries[i]`.

**Note**

*   For each query, treat <code>s[l<sub>i</sub>...r<sub>i</sub>]</code> as if it is **augmented** with a `'1'` at both ends, forming <code>t = '1' + s[l<sub>i</sub>...r<sub>i</sub>] + '1'</code>. The augmented `'1'`s **do not** contribute to the final count.
*   The queries are independent of each other.

**Example 1:**

**Input:** s = "01", queries = \[\[0,1]]

**Output:** [1]

**Explanation:**

Because there is no block of `'1'`s surrounded by `'0'`s, no valid trade is possible. The maximum number of active sections is 1.

**Example 2:**

**Input:** s = "0100", queries = \[\[0,3],[0,2],[1,3],[2,3]]

**Output:** [4,3,1,1]

**Explanation:**

*   Query `[0, 3]` → Substring `"0100"` → Augmented to `"101001"`   
     Choose `"0100"`, convert `"0100"` → `"0000"` → `"1111"`.   
     The final string without augmentation is `"1111"`. The maximum number of active sections is 4.
    
*   Query `[0, 2]` → Substring `"010"` → Augmented to `"10101"`   
     Choose `"010"`, convert `"010"` → `"000"` → `"111"`.   
     The final string without augmentation is `"1110"`. The maximum number of active sections is 3.
    
*   Query `[1, 3]` → Substring `"100"` → Augmented to `"11001"`   
     Because there is no block of `'1'`s surrounded by `'0'`s, no valid trade is possible. The maximum number of active sections is 1.
    
*   Query `[2, 3]` → Substring `"00"` → Augmented to `"1001"`   
     Because there is no block of `'1'`s surrounded by `'0'`s, no valid trade is possible. The maximum number of active sections is 1.
    

**Example 3:**

**Input:** s = "1000100", queries = \[\[1,5],[0,6],[0,4]]

**Output:** [6,7,2]

**Explanation:**

*   Query `[1, 5]` → Substring `"00010"` → Augmented to `"1000101"`   
     Choose `"00010"`, convert `"00010"` → `"00000"` → `"11111"`.   
     The final string without augmentation is `"1111110"`. The maximum number of active sections is 6.
    
*   Query `[0, 6]` → Substring `"1000100"` → Augmented to `"110001001"`   
     Choose `"000100"`, convert `"000100"` → `"000000"` → `"111111"`.   
     The final string without augmentation is `"1111111"`. The maximum number of active sections is 7.
    
*   Query `[0, 4]` → Substring `"10001"` → Augmented to `"1100011"`   
     Because there is no block of `'1'`s surrounded by `'0'`s, no valid trade is possible. The maximum number of active sections is 2.
    

**Example 4:**

**Input:** s = "01010", queries = \[\[0,3],[1,4],[1,3]]

**Output:** [4,4,2]

**Explanation:**

*   Query `[0, 3]` → Substring `"0101"` → Augmented to `"101011"`   
     Choose `"010"`, convert `"010"` → `"000"` → `"111"`.   
     The final string without augmentation is `"11110"`. The maximum number of active sections is 4.
    
*   Query `[1, 4]` → Substring `"1010"` → Augmented to `"110101"`   
     Choose `"010"`, convert `"010"` → `"000"` → `"111"`.   
     The final string without augmentation is `"01111"`. The maximum number of active sections is 4.
    
*   Query `[1, 3]` → Substring `"101"` → Augmented to `"11011"`   
     Because there is no block of `'1'`s surrounded by `'0'`s, no valid trade is possible. The maximum number of active sections is 2.
    

**Constraints:**

*   <code>1 <= n == s.length <= 10<sup>5</sup></code>
*   <code>1 <= queries.length <= 10<sup>5</sup></code>
*   `s[i]` is either `'0'` or `'1'`.
*   <code>queries[i] = [l<sub>i</sub>, r<sub>i</sub>]</code>
*   <code>0 <= l<sub>i</sub> <= r<sub>i</sub> < n</code>

## Solution

```kotlin
import kotlin.math.max

class Solution {
    fun maxActiveSectionsAfterTrade(s: String, queries: Array<IntArray>): List<Int> {
        val n = s.length
        var activeCount = 0
        for (ch in s.toCharArray()) {
            if (ch == '1') {
                activeCount++
            }
        }
        val segments: MutableList<IntArray?> = ArrayList<IntArray?>()
        var start = 0
        for (i in 0..<n) {
            if (i == n - 1 || s[i] != s[i + 1]) {
                segments.add(intArrayOf(start, i - start + 1))
                start = i + 1
            }
        }
        val segmentCount = segments.size
        val maxPower = 20
        val rmq = Array<IntArray>(maxPower) { IntArray(segmentCount) }
        for (i in 0..<maxPower) {
            rmq[i].fill(NEG_INF)
        }
        for (i in 0..<segmentCount) {
            if (s[segments[i]!![0]] == '0' && i + 2 < segmentCount) {
                rmq[0][i] = segments[i]!![1] + segments[i + 2]!![1]
            }
        }
        var power = 1
        var rangeLen = 2
        while (power < maxPower) {
            var i = 0
            var j = rangeLen - 1
            while (j < segmentCount) {
                rmq[power][i] =
                    max(rmq[power - 1][i], rmq[power - 1][i + rangeLen / 2])
                i++
                j++
            }
            power++
            rangeLen *= 2
        }
        val result: MutableList<Int> = ArrayList<Int>()
        for (query in queries) {
            val left = query[0]
            val right = query[1]
            val leftIndex = binarySearch(segments, left) - 1
            val rightIndex = binarySearch(segments, right) - 1
            if (rightIndex - leftIndex + 1 <= 2) {
                result.add(activeCount)
                continue
            }
            var bestIncrease = max(getMaxInRange(rmq, leftIndex + 1, rightIndex - 3), 0)
            bestIncrease = max(
                bestIncrease,
                calculateNewSections(
                    s, segments, left, right, leftIndex, rightIndex, leftIndex,
                ),
            )
            bestIncrease = max(
                bestIncrease,
                calculateNewSections(
                    s,
                    segments,
                    left,
                    right,
                    leftIndex,
                    rightIndex,
                    rightIndex - 2,
                ),
            )
            result.add(activeCount + bestIncrease)
        }
        return result
    }

    private fun binarySearch(segments: MutableList<IntArray?>, key: Int): Int {
        var lo = 0
        var hi = segments.size
        while (lo < hi) {
            val mid = lo + (hi - lo) / 2
            if (segments[mid]!![0] > key) {
                hi = mid
            } else {
                lo = mid + 1
            }
        }
        return lo
    }

    private fun getMaxInRange(rmq: Array<IntArray>, left: Int, right: Int): Int {
        if (left > right) {
            return NEG_INF
        }
        val power = 31 - Integer.numberOfLeadingZeros(right - left + 1)
        return max(rmq[power][left], rmq[power][right - (1 shl power) + 1])
    }

    private fun getSegmentSize(
        segments: MutableList<IntArray?>,
        left: Int,
        right: Int,
        leftIndex: Int,
        rightIndex: Int,
        i: Int,
    ): Int {
        if (i == leftIndex) {
            return segments[leftIndex]!![1] - (left - segments[leftIndex]!![0])
        }
        if (i == rightIndex) {
            return right - segments[rightIndex]!![0] + 1
        }
        return segments[i]!![1]
    }

    private fun calculateNewSections(
        s: String,
        segments: MutableList<IntArray?>,
        left: Int,
        right: Int,
        leftIndex: Int,
        rightIndex: Int,
        i: Int,
    ): Int {
        if (i < 0 || i + 2 >= segments.size || s[segments[i]!![0]] == '1') {
            return NEG_INF
        }
        val size1 = getSegmentSize(segments, left, right, leftIndex, rightIndex, i)
        val size2 = getSegmentSize(segments, left, right, leftIndex, rightIndex, i + 2)
        return size1 + size2
    }

    companion object {
        private const val INF = 1e9
        private const val NEG_INF: Int = -INF.toInt()
    }
}
```