[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3569\. Maximize Count of Distinct Primes After Split

Hard

You are given an integer array `nums` having length `n` and a 2D integer array `queries` where `queries[i] = [idx, val]`.

For each query:

1.  Update `nums[idx] = val`.
2.  Choose an integer `k` with `1 <= k < n` to split the array into the non-empty prefix `nums[0..k-1]` and suffix `nums[k..n-1]` such that the sum of the counts of **distinct** prime values in each part is **maximum**.

**Note:** The changes made to the array in one query persist into the next query.

Return an array containing the result for each query, in the order they are given.

**Example 1:**

**Input:** nums = [2,1,3,1,2], queries = \[\[1,2],[3,3]]

**Output:** [3,4]

**Explanation:**

*   Initially `nums = [2, 1, 3, 1, 2]`.
*   After 1<sup>st</sup> query, `nums = [2, 2, 3, 1, 2]`. Split `nums` into `[2]` and `[2, 3, 1, 2]`. `[2]` consists of 1 distinct prime and `[2, 3, 1, 2]` consists of 2 distinct primes. Hence, the answer for this query is `1 + 2 = 3`.
*   After 2<sup>nd</sup> query, `nums = [2, 2, 3, 3, 2]`. Split `nums` into `[2, 2, 3]` and `[3, 2]` with an answer of `2 + 2 = 4`.
*   The output is `[3, 4]`.

**Example 2:**

**Input:** nums = [2,1,4], queries = \[\[0,1]]

**Output:** [0]

**Explanation:**

*   Initially `nums = [2, 1, 4]`.
*   After 1<sup>st</sup> query, `nums = [1, 1, 4]`. There are no prime numbers in `nums`, hence the answer for this query is 0.
*   The output is `[0]`.

**Constraints:**

*   <code>2 <= n == nums.length <= 5 * 10<sup>4</sup></code>
*   <code>1 <= queries.length <= 5 * 10<sup>4</sup></code>
*   <code>1 <= nums[i] <= 10<sup>5</sup></code>
*   `0 <= queries[i][0] < nums.length`
*   <code>1 <= queries[i][1] <= 10<sup>5</sup></code>

## Solution

```kotlin
import java.util.TreeSet
import kotlin.math.max
import kotlin.math.min

class Solution {
    private class Node {
        var maxVal: Int = 0
        var lazyDelta: Int = 0
    }

    private class SegmentTree(var n: Int) {
        var tree: Array<Node>

        init {
            tree = Array<Node>(4 * this.n) { Node() }
        }

        fun push(nodeIdx: Int) {
            if (tree[nodeIdx].lazyDelta != 0) {
                tree[2 * nodeIdx].maxVal += tree[nodeIdx].lazyDelta
                tree[2 * nodeIdx].lazyDelta += tree[nodeIdx].lazyDelta
                tree[2 * nodeIdx + 1].maxVal += tree[nodeIdx].lazyDelta
                tree[2 * nodeIdx + 1].lazyDelta += tree[nodeIdx].lazyDelta
                tree[nodeIdx].lazyDelta = 0
            }
        }

        fun update(queryStart: Int, queryEnd: Int, delta: Int) {
            var queryStart = queryStart
            var queryEnd = queryEnd
            queryStart = max(1, queryStart)
            queryEnd = min(n - 1, queryEnd)
            if (queryStart > queryEnd) {
                return
            }
            update(1, 1, n - 1, queryStart, queryEnd, delta)
        }

        fun update(
            nodeIdx: Int,
            start: Int,
            end: Int,
            queryStart: Int,
            queryEnd: Int,
            delta: Int,
        ) {
            if (start > end || start > queryEnd || end < queryStart) {
                return
            }
            if (queryStart <= start && end <= queryEnd) {
                tree[nodeIdx].maxVal += delta
                tree[nodeIdx].lazyDelta += delta
                return
            }
            push(nodeIdx)

            val mid = (start + end) / 2
            update(2 * nodeIdx, start, mid, queryStart, queryEnd, delta)
            update(2 * nodeIdx + 1, mid + 1, end, queryStart, queryEnd, delta)
            tree[nodeIdx].maxVal = max(tree[2 * nodeIdx].maxVal, tree[2 * nodeIdx + 1].maxVal)
        }

        fun queryMax(): Int {
            if (n - 1 < 1) {
                return 0
            }
            return tree[1].maxVal
        }
    }

    fun maximumCount(nums: IntArray, queries: Array<IntArray>): IntArray {
        val n = nums.size
        val primeIndices: MutableMap<Int, TreeSet<Int>> = HashMap()
        for (i in 0..<n) {
            if (isPrime[nums[i]]) {
                primeIndices.computeIfAbsent(nums[i]) { _: Int -> TreeSet<Int>() }.add(i)
            }
        }
        val segmentTree = SegmentTree(n)
        for (entry in primeIndices.entries) {
            val indices = entry.value
            val first: Int = indices.first()!!
            val last: Int = indices.last()!!
            segmentTree.update(first + 1, last, 1)
        }
        val result = IntArray(queries.size)
        for (q in queries.indices) {
            val idx = queries[q][0]
            val `val` = queries[q][1]
            val oldVal = nums[idx]
            if (isPrime[oldVal]) {
                val indices: TreeSet<Int> = primeIndices[oldVal]!!
                val oldFirst: Int = indices.first()!!
                val oldLast: Int = indices.last()!!
                indices.remove(idx)
                if (indices.isEmpty()) {
                    primeIndices.remove(oldVal)
                    segmentTree.update(oldFirst + 1, oldLast, -1)
                } else {
                    val newFirst: Int = indices.first()!!
                    val newLast: Int = indices.last()!!

                    if (idx == oldFirst && newFirst != oldFirst) {
                        segmentTree.update(oldFirst + 1, newFirst, -1)
                    }
                    if (idx == oldLast && newLast != oldLast) {
                        segmentTree.update(newLast + 1, oldLast, -1)
                    }
                }
            }
            nums[idx] = `val`
            if (isPrime[`val`]) {
                val wasNewPrime = !primeIndices.containsKey(`val`)
                val indices = primeIndices.computeIfAbsent(`val`) { _: Int -> TreeSet<Int>() }
                val oldFirst: Int = (if (indices.isEmpty()) -1 else indices.first())!!
                val oldLast: Int = (if (indices.isEmpty()) -1 else indices.last())!!
                indices.add(idx)
                val newFirst: Int = indices.first()!!
                val newLast: Int = indices.last()!!
                if (wasNewPrime) {
                    segmentTree.update(newFirst + 1, newLast, 1)
                } else {
                    if (idx < oldFirst) {
                        segmentTree.update(newFirst + 1, oldFirst, 1)
                    }
                    if (idx > oldLast) {
                        segmentTree.update(oldLast + 1, newLast, 1)
                    }
                }
            }
            val totalDistinctPrimesInCurrentNums = primeIndices.size
            var maxIntersection = segmentTree.queryMax()
            maxIntersection = max(0, maxIntersection)
            result[q] = totalDistinctPrimesInCurrentNums + maxIntersection
        }
        return result
    }

    companion object {
        private const val MAX_VAL = 100005
        private val isPrime = BooleanArray(MAX_VAL)

        init {
            isPrime.fill(true)
            isPrime[1] = false
            isPrime[0] = false
            var i = 2
            while (i * i < MAX_VAL) {
                if (isPrime[i]) {
                    var j = i * i
                    while (j < MAX_VAL) {
                        isPrime[j] = false
                        j += i
                    }
                }
                i++
            }
        }
    }
}
```