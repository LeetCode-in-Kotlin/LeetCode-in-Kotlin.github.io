[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3943\. Number of Pairs After Increment

Hard

You are given two integer arrays `nums1` and `nums2`, and a 2D integer array `queries`.

Each `queries[i]` is one of the following types:

*   `[1, x, y, val]` – **Add** `val` to every element in `nums2[x..y]`.
*   `[2, tot]` – **Compute** the number of pairs `(j, k)` such that `nums1[j] + nums2[k] == tot`.

Return an integer array `answer`, where `answer[j]` is the number of pairs for the <code>j<sup>th</sup></code> query of type 2.

**Example 1:**

**Input:** nums1 = [1,2], nums2 = [3,4], queries = \[\[2,5],[1,0,0,2],[2,5]]

**Output:** [2,1]

**Explanation:**

*   `queries[0] = [2, 5]`: Valid pairs are `nums1[0] + nums2[1] = 1 + 4 = 5` and `nums1[1] + nums2[0] = 2 + 3 = 5`.
*   `queries[1] = [1, 0, 0, 2]`: Add 2 to `nums2[0]`, resulting in `nums2 = [5, 4]`.
*   `queries[2] = [2, 5]`: Valid pair is `nums1[0] + nums2[1] = 1 + 4 = 5`.
*   Thus, the `answer = [2, 1]`.

**Example 2:**

**Input:** nums1 = [1,1], nums2 = [2,2,3], queries = \[\[2,4],[1,0,1,1],[2,4]]

**Output:** [2,6]

**Explanation:**

*   `queries[0] = [2, 4]`: Valid pairs are `nums1[0] + nums2[2] = 1 + 3` and `nums1[1] + nums2[2] = 1 + 3`.
*   `queries[1] = [1, 0, 1, 1]`: Add 1 to `nums2[0]` and `nums2[1]`, resulting in `nums2 = [3, 3, 3]`.
*   `queries[2] = [2, 4]`: Every element of `nums1 = [1, 1]` pairs with every element of `nums2 = [3, 3, 3]` as `1 + 3 = 4`. That gives `2 × 3 = 6` pairs in total.
*   Thus, the `answer = [2, 6]`.

**Example 3:**

**Input:** nums1 = [2,5,8,4], nums2 = [1,3,8], queries = \[\[2,9],[1,1,2,1],[2,10]]

**Output:** [1,0]

**Explanation:**

*   `queries[0] = [2, 9]`: Only valid pair is `nums1[2] + nums2[0] = 8 + 1 = 9`.
*   `queries[1] = [1, 1, 2, 1]`: Add 1 to `nums2[1]` and `nums2[2]`, resulting in `nums2 = [1, 4, 9]`.
*   `queries[2] = [2, 10]`: No pair sums to `10`.
*   Thus, the `answer = [1, 0]`.

**Constraints:**

*   `1 <= nums1.length <= 5`
*   <code>1 <= nums2.length <= 5 * 10<sup>4</sup></code>
*   <code>1 <= nums1[i], nums2[i] <= 10<sup>5</sup></code>
*   <code>1 <= queries.length <= 5 * 10<sup>4</sup></code>
*   `queries[i].length == 2 or 4`
    *   `queries[i] == [1, x, y, val], or`
    *   `queries[i] == [2, tot]`
    *   `0 <= x <= y < nums2.length`
    *   <code>1 <= val <= 10<sup>5</sup></code>
    *   <code>1 <= tot <= 10<sup>9</sup></code>

## Solution

```kotlin
class Solution {
    private lateinit var cache: HashMap<Long, Long>

    private fun update(nums2: LongArray, l: Int, r: Int, `val`: Long) {
        if (r < l) {
            return
        }
        for (i in l..r) {
            val oldValue = nums2[i]
            val newValue = nums2[i] + `val`
            cache[oldValue] = cache.getOrDefault(oldValue, 0L) - 1
            cache[newValue] = cache.getOrDefault(newValue, 0L) + 1
            nums2[i] = newValue
        }
    }

    fun numberOfPairs(nums1: IntArray, nums2: IntArray, queries: Array<IntArray>): IntArray {
        val n2 = initialize(nums2)
        var globalOffset: Long = 0
        val answers: MutableList<Int> = ArrayList<Int>()
        var q = 0
        while (q < queries.size) {
            val result = processQuery(q, queries, nums1, n2, globalOffset)
            q = result.nextIndex
            globalOffset = result.globalOffset
            if (result.answer != null) {
                answers.add(result.answer)
            }
        }
        return toArray(answers)
    }

    private fun initialize(nums2: IntArray): LongArray {
        cache = HashMap<Long, Long>()
        val result = LongArray(nums2.size)
        for (i in nums2.indices) {
            result[i] = nums2[i].toLong()
            incrementCount(result[i])
        }
        return result
    }

    private fun incrementCount(value: Long) {
        cache[value] = cache.getOrDefault(value, 0L) + 1
    }

    private fun processQuery(
        index: Int,
        queries: Array<IntArray>,
        nums1: IntArray,
        nums2: LongArray,
        globalOffset: Long,
    ): QueryResult {
        val query = queries[index]
        if (query[0] == 1) {
            return processUpdateQuery(index, queries, nums2, globalOffset)
        }
        val answer = countPairs(nums1, query[1].toLong(), globalOffset)
        return QueryResult(index + 1, globalOffset, answer)
    }

    private fun processUpdateQuery(
        index: Int,
        queries: Array<IntArray>,
        nums2: LongArray,
        globalOffset: Long,
    ): QueryResult {
        var globalOffset = globalOffset
        val query = queries[index]
        val l = query[1]
        val r = query[2]
        var `val` = query[3].toLong()
        var nextIndex = index + 1
        while (nextIndex < queries.size && hasSameRange(queries[nextIndex], l, r)) {
            `val` += queries[nextIndex][3].toLong()
            nextIndex++
        }
        globalOffset = applyUpdate(nums2, l, r, `val`, globalOffset)
        return QueryResult(nextIndex, globalOffset, null)
    }

    private fun hasSameRange(query: IntArray, l: Int, r: Int): Boolean =
        query.size == 4 && query[1] == l && query[2] == r

    private fun applyUpdate(nums2: LongArray, l: Int, r: Int, `val`: Long, globalOffset: Long): Long {
        val length = r - l + 1
        if (length <= nums2.size / 2) {
            update(nums2, l, r, `val`)
            return globalOffset
        }
        update(nums2, 0, l - 1, -`val`)
        update(nums2, r + 1, nums2.size - 1, -`val`)
        return globalOffset + `val`
    }

    private fun countPairs(nums1: IntArray, target: Long, globalOffset: Long): Int {
        var totalPairs: Long = 0
        for (num1 in nums1) {
            val num2 = target - num1 - globalOffset
            totalPairs += cache.getOrDefault(num2, 0L)
        }
        return totalPairs.toInt()
    }

    private fun toArray(answers: MutableList<Int>): IntArray {
        val result = IntArray(answers.size)
        for (i in answers.indices) {
            result[i] = answers[i]
        }
        return result
    }

    private class QueryResult(val nextIndex: Int, val globalOffset: Long, val answer: Int?)
}
```