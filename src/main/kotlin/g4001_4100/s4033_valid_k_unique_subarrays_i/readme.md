[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 4033\. Valid K-Unique Subarrays I

Hard

You are given an integer array `nums` and an integer `k`.

You are also given a 2D integer array `queries`, where <code>queries[i] = [l<sub>i</sub>, r<sub>i</sub>]</code> represents the non-empty subarrays <code>nums[l<sub>i</sub>..r<sub>i</sub>]</code>.

For each query, the **subarray** <code>nums[l<sub>i</sub>..r<sub>i</sub>]</code> is considered **valid** if:

*   It contains **exactly** `k` **distinct** numbers, and
*   The **frequency** of every number in the **subarray** is **even**.

Return a boolean array `ans`, where `ans[i]` is `true` if <code>nums[l<sub>i</sub>..r<sub>i</sub>]</code> is **valid**, and `false` otherwise.

**Example 1:**

**Input:** nums = [1,2,2,1], k = 2, queries = \[\[0,1],[0,3],[1,2]]

**Output:** [false,true,false]

**Explanation:**

| `i` | <code>[l<sub>i</sub>, r<sub>i</sub>]</code> | Subarray | Unique numbers | Frequency | Validity check |
| --- | --- | --- | --- | --- | --- |
| 0 | [0, 1] | [1, 2] | {1, 2} → 2 | {1: 1, 2: 1} | `false`: Element counts are not even. |
| 1 | [0, 3] | [1, 2, 2, 1] | {1, 2} → 2 | {1: 2, 2: 2} | `true`: Exactly `k = 2` distinct elements, all appear an even number of times. |
| 2 | [1, 2] | [2, 2] | {2} → 1 | {2: 2} | `false`: Number of distinct elements is less than `k = 2`. |

Thus, `ans = [false, true, false]`.

**Example 2:**

**Input:** nums = [3,3,3], k = 1, queries = \[\[1,2],[0,2]]

**Output:** [true,false]

**Explanation:**

| `i` | <code>[l<sub>i</sub>, r<sub>i</sub>]</code> | Subarray | Unique numbers | Frequency | Validity check |
| --- | --- | --- | --- | --- | --- |
| 0 | [1, 2] | [3, 3] | {3} → 1 | {3: 2} | `true`: Exactly `k = 1` distinct element, appears an even number of times. |
| 1 | [0, 2] | [3, 3, 3] | {3} → 1 | {3: 3} | `false`: 3 does not appear an even number of times. |

Thus, `ans = [true, false]`.

**Constraints:**

*   <code>2 <= n == nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>5</sup></code>
*   `1 <= k <= n`
*   <code>1 <= queries.length <= 10<sup>5</sup></code>
*   <code>queries[i] == [l<sub>i</sub>, r<sub>i</sub>]</code>
*   <code>0 <= l<sub>i</sub> < r<sub>i</sub> <= n - 1</code>

## Solution

```kotlin
import kotlin.math.max
import kotlin.math.sqrt

class Solution {
    private var countOdd = 0
    private var countDistinct = 0

    fun validSubarrays(nums: IntArray, k: Int, queries: Array<IntArray>): BooleanArray {
        var maxNum = Int.MIN_VALUE
        val prefixXor = IntArray(nums.size + 1)
        for (i in nums.indices) {
            maxNum = max(maxNum, nums[i])
            prefixXor[i + 1] = prefixXor[i] xor nums[i]
        }
        val sortedQueries: Array<IntArray> = Array<IntArray>(queries.size) { IntArray(0) }
        val queriesCount = this.sortQueries(nums, k, prefixXor, queries, sortedQueries)
        val result = BooleanArray(queries.size)
        if (queriesCount == 0) {
            return result
        }
        var currentLeft = sortedQueries[0][0]
        var currentRight = sortedQueries[0][0] - 1
        countOdd = 0
        countDistinct = 0
        val counts = IntArray(maxNum + 1)
        for (i in 0..<queriesCount) {
            val query: IntArray = sortedQueries[i]
            while (currentLeft > query[0]) {
                this.add(counts, nums, --currentLeft)
            }
            while (currentRight < query[1]) {
                this.add(counts, nums, ++currentRight)
            }
            while (currentLeft < query[0]) {
                this.remove(counts, nums, currentLeft++)
            }
            while (currentRight > query[1]) {
                this.remove(counts, nums, currentRight--)
            }
            result[query[2]] = countDistinct == k && countOdd == 0
        }
        return result
    }

    private fun sortQueries(
        nums: IntArray,
        k: Int,
        prefixXor: IntArray,
        queries: Array<IntArray>,
        sortedQueries: Array<IntArray>,
    ): Int {
        val blockSize = max(1, (nums.size / sqrt(queries.size.toDouble())).toInt())
        var queriesCount = 0
        for (i in queries.indices) {
            val query = queries[i]
            val length = query[1] - query[0]
            if (length % 2 == 1 && length + 1 >= 2 * k && (prefixXor[query[1] + 1] xor prefixXor[query[0]]) == 0) {
                sortedQueries[queriesCount++] =
                    intArrayOf(query[0], query[1], i, query[0] / blockSize)
            }
        }
        sortedQueries.sortWith(
            { o1: IntArray?, o2: IntArray? ->
                return@sortWith if (o1!![3] == o2!![3]) {
                    if (o1[3] % 2 == 0) o1[1] - o2[1] else o2[1] - o1[1]
                } else {
                    o1[3].compareTo(o2[3])
                }
            },
            0,
            queriesCount,
        )
        return queriesCount
    }

    private fun add(counts: IntArray, nums: IntArray, index: Int) {
        val count = ++counts[nums[index]]
        if (count == 1) {
            countDistinct++
        }
        countOdd += 2 * (count and 1) - 1
    }

    private fun remove(counts: IntArray, nums: IntArray, index: Int) {
        val count = --counts[nums[index]]
        if (count == 0) {
            countDistinct--
        }
        countOdd += 2 * (count and 1) - 1
    }
}
```