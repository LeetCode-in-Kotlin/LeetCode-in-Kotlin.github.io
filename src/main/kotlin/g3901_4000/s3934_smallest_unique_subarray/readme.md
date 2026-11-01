[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3934\. Smallest Unique Subarray

Hard

You are given an integer array `nums`.

Find the **minimum** length of a subarray that is **not** **identical** to any other **subarray** in `nums`.

Return an integer denoting the **minimum possible length** of such a **subarray**.

Two **subarrays** are considered identical if they have the same length and the same elements in corresponding positions.

**Example 1:**

**Input:** nums = [3,3,3]

**Output:** 3

**Explanation:**

*   Subarrays of length 1: `[3]` → appears 3 times
*   Subarrays of length 2: `[3, 3]` → appears 2 times
*   Subarrays of length 3: `[3, 3, 3]` → appears once

The subarray `[3, 3, 3]` is unique, so the smallest unique subarray length is 3.

**Example 2:**

**Input:** nums = [2,1,2,3,3]

**Output:** 1

**Explanation:**

Subarrays of length 1:

*   `[2]` → appears 2 times
*   `[1]` → appears once
*   `[3]` → appears 2 times

The subarray `[1]` is unique, so the smallest unique subarray length is 1.

**Example 3:**

**Input:** nums = [1,1,2,2,1]

**Output:** 2

**Explanation:**

Subarrays of length 1:

*   `[1]` → appears 3 times
*   `[2]` → appears 2 times

Subarrays of length 2:

*   `[1, 1]` → appears once
*   `[1, 2]` → appears once
*   `[2, 2]` → appears once
*   `[2, 1]` → appears once

There is at least one subarray of length 2 that is unique, so the smallest unique subarray length is 2.

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>5</sup></code>

## Solution

```kotlin
class Solution {
    fun smallestUniqueSubarray(nums: IntArray): Int {
        val n = nums.size
        if (n == 1) {
            return 1
        }
        val sa = buildSuffixArray(nums)
        val lcp = buildLCP(nums, sa)
        return findSmallestUniqueLength(sa, lcp, n)
    }

    private fun findSmallestUniqueLength(sa: IntArray, lcp: IntArray, n: Int): Int {
        var ans = n
        for (rank in 0 until n) {
            val maxLcp = getMaxLcp(rank, lcp, n)
            val suffixLength = n - sa[rank]
            val uniqueLength = maxLcp + 1

            if (uniqueLength <= suffixLength) {
                ans = minOf(ans, uniqueLength)
            }
        }
        return ans
    }

    private fun getMaxLcp(rank: Int, lcp: IntArray, n: Int): Int {
        var maxLcp = 0
        if (rank > 0) {
            maxLcp = maxOf(maxLcp, lcp[rank - 1])
        }
        if (rank < n - 1) {
            maxLcp = maxOf(maxLcp, lcp[rank])
        }
        return maxLcp
    }

    // ------------------------------------------------------------
    // Build suffix array using prefix doubling + counting sort
    // ------------------------------------------------------------
    private fun buildSuffixArray(nums: IntArray): IntArray {
        val n = nums.size
        val m = n + 1
        val a = addSentinel(nums)
        val data = initializeSuffixArray(a, m)
        var len = 1
        while (len < m && data.classes < m) {
            buildNextSuffixArray(data, len, m)
            if (shouldStopDoubling(len, m)) {
                break
            }
            len *= 2
        }
        return removeSentinel(data.sa, n)
    }

    private fun addSentinel(nums: IntArray): IntArray = IntArray(nums.size + 1) { i ->
        if (i < nums.size) nums[i] else 0
    }

    private class SuffixArrayData(
        var sa: IntArray,
        var rank: IntArray,
        var tmpRank: IntArray,
        val tmpSa: IntArray,
        val count: IntArray,
        var classes: Int,
    )

    private fun initializeSuffixArray(a: IntArray, m: Int): SuffixArrayData {
        val sa = IntArray(m)
        val rank = IntArray(m)
        val tmpRank = IntArray(m)
        val tmpSa = IntArray(m)
        val maxValue = maxOf(100000, m) + 1
        val count = IntArray(maxValue)
        sortInitialValues(a, sa, count)
        val classes = assignInitialRanks(a, sa, rank)
        return SuffixArrayData(sa, rank, tmpRank, tmpSa, count, classes)
    }

    private fun sortInitialValues(a: IntArray, sa: IntArray, count: IntArray) {
        for (x in a) {
            count[x]++
        }
        buildPrefixCounts(count)
        for (i in a.size - 1 downTo 0) {
            sa[--count[a[i]]] = i
        }
    }

    private fun buildPrefixCounts(count: IntArray) {
        for (i in 1 until count.size) {
            count[i] += count[i - 1]
        }
    }

    private fun buildPrefixCounts(count: IntArray, classes: Int) {
        for (i in 1 until classes) {
            count[i] += count[i - 1]
        }
    }

    private fun assignInitialRanks(a: IntArray, sa: IntArray, rank: IntArray): Int {
        var classes = 1
        rank[sa[0]] = 0
        for (i in 1 until a.size) {
            if (a[sa[i]] != a[sa[i - 1]]) {
                classes++
            }
            rank[sa[i]] = classes - 1
        }
        return classes
    }

    private fun buildNextSuffixArray(data: SuffixArrayData, len: Int, m: Int): SuffixArrayData {
        shiftSuffixes(data.sa, data.tmpSa, len, m)
        countingSortByRank(data, m)
        val newClasses = assignNewRanks(data, len, m)
        val swap = data.rank
        data.rank = data.tmpRank
        data.tmpRank = swap
        data.classes = newClasses
        return data
    }

    private fun shiftSuffixes(sa: IntArray, tmpSa: IntArray, len: Int, m: Int) {
        for (i in 0 until m) {
            var shifted = sa[i] - len
            if (shifted < 0) {
                shifted += m
            }
            tmpSa[i] = shifted
        }
    }

    private fun countingSortByRank(data: SuffixArrayData, m: Int) {
        for (i in 0 until data.classes) {
            data.count[i] = 0
        }
        for (i in 0 until m) {
            data.count[data.rank[data.tmpSa[i]]]++
        }
        buildPrefixCounts(data.count, data.classes)
        for (i in m - 1 downTo 0) {
            val x = data.tmpSa[i]
            data.sa[--data.count[data.rank[x]]] = x
        }
    }

    private fun assignNewRanks(data: SuffixArrayData, len: Int, m: Int): Int {
        data.tmpRank[data.sa[0]] = 0
        var newClasses = 1
        for (i in 1 until m) {
            val cur = data.sa[i]
            val prev = data.sa[i - 1]
            if (differentRanks(data.rank, cur, prev, len, m)) {
                newClasses++
            }
            data.tmpRank[cur] = newClasses - 1
        }
        return newClasses
    }

    private fun differentRanks(rank: IntArray, cur: Int, prev: Int, len: Int, m: Int): Boolean {
        val curSecond = (cur + len) % m
        val prevSecond = (prev + len) % m
        return rank[cur] != rank[prev] || rank[curSecond] != rank[prevSecond]
    }

    private fun shouldStopDoubling(len: Int, m: Int): Boolean = len > m / 2

    private fun removeSentinel(sa: IntArray, n: Int): IntArray {
        val result = IntArray(n)
        var idx = 0
        for (x in sa) {
            if (x != n) {
                result[idx++] = x
            }
        }
        return result
    }

    private fun buildLCP(nums: IntArray, sa: IntArray): IntArray {
        val n = nums.size
        val rank = buildRanks(sa, n)
        val lcp = IntArray(n - 1)
        buildLcpValues(nums, sa, rank, lcp, n)
        return lcp
    }

    private fun buildRanks(sa: IntArray, n: Int): IntArray {
        val rank = IntArray(n)
        for (i in 0 until n) {
            rank[sa[i]] = i
        }
        return rank
    }

    private fun buildLcpValues(nums: IntArray, sa: IntArray, rank: IntArray, lcp: IntArray, n: Int) {
        var k = 0
        for (i in 0 until n) {
            val r = rank[i]
            if (r == n - 1) {
                k = 0
                continue
            }
            val j = sa[r + 1]
            k = calculateLcp(nums, i, j, k)
            lcp[r] = k
            if (k > 0) {
                k--
            }
        }
    }

    private fun calculateLcp(nums: IntArray, i: Int, j: Int, k: Int): Int {
        var curK = k
        while (i + curK < nums.size && j + curK < nums.size && nums[i + curK] == nums[j + curK]) {
            curK++
        }
        return curK
    }
}
```