[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3624\. Number of Integers With Popcount-Depth Equal to K II

Hard

You are given an integer array `nums`.

For any positive integer `x`, define the following sequence:

*   <code>p<sub>0</sub> = x</code>
*   <code>p<sub>i+1</sub> = popcount(p<sub>i</sub>)</code> for all `i >= 0`, where `popcount(y)` is the number of set bits (1's) in the binary representation of `y`.

This sequence will eventually reach the value 1.

The **popcount-depth** of `x` is defined as the **smallest** integer `d >= 0` such that <code>p<sub>d</sub> = 1</code>.

For example, if `x = 7` (binary representation `"111"`). Then, the sequence is: `7 → 3 → 2 → 1`, so the popcount-depth of 7 is 3.

You are also given a 2D integer array `queries`, where each `queries[i]` is either:

*   `[1, l, r, k]` - **Determine** the number of indices `j` such that `l <= j <= r` and the **popcount-depth** of `nums[j]` is equal to `k`.
*   `[2, idx, val]` - **Update** `nums[idx]` to `val`.

Return an integer array `answer`, where `answer[i]` is the number of indices for the <code>i<sup>th</sup></code> query of type `[1, l, r, k]`.

**Example 1:**

**Input:** nums = [2,4], queries = \[\[1,0,1,1],[2,1,1],[1,0,1,0]]

**Output:** [2,1]

**Explanation:**

| `i` | `queries[i]` | `nums`   | binary(`nums`) | popcount-<br>depth  | `[l, r]` | `k` | Valid<br>`nums[j]`  | updated<br>`nums`  | Answer  |
|-----|--------------|----------|----------------|---------------------|----------|-----|---------------------|--------------------|---------|
| 0   | [1,0,1,1]    | [2,4]    | [10, 100]      | [1, 1]              | [0, 1]   | 1   | [0, 1]              | —                  | 2       |
| 1   | [2,1,1]      | [2,4]    | [10, 100]      | [1, 1]              | —        | —   | —                   | [2,1]              | —       |
| 2   | [1,0,1,0]    | [2,1]    | [10, 1]        | [1, 0]              | [0, 1]   | 0   | [1]                 | —                  | 1       |

Thus, the final `answer` is `[2, 1]`.

**Example 2:**

**Input:** nums = [3,5,6], queries = \[\[1,0,2,2],[2,1,4],[1,1,2,1],[1,0,1,0]]

**Output:** [3,1,0]

**Explanation:**

| `i` | `queries[i]`   | `nums`         | binary(`nums`)       | popcount-<br>depth | `[l, r]` | `k` | Valid<br>`nums[j]` | updated<br>`nums` | Answer |
|-----|----------------|----------------|-----------------------|---------------------|----------|-----|---------------------|--------------------|---------|
| 0   | [1,0,2,2]      | [3, 5, 6]      | [11, 101, 110]        | [2, 2, 2]           | [0, 2]   | 2   | [0, 1, 2]          | —                  | 3       |
| 1   | [2,1,4]        | [3, 5, 6]      | [11, 101, 110]        | [2, 2, 2]           | —        | —   | —                   | [3, 4, 6]          | —       |
| 2   | [1,1,2,1]      | [3, 4, 6]      | [11, 100, 110]        | [2, 1, 2]           | [1, 2]   | 1   | [1]                | —                  | 1       |
| 3   | [1,0,1,0]      | [3, 4, 6]      | [11, 100, 110]        | [2, 1, 2]           | [0, 1]   | 0   | []                 | —                  | 0       |

Thus, the final `answer` is `[3, 1, 0]`.

**Example 3:**

**Input:** nums = [1,2], queries = \[\[1,0,1,1],[2,0,3],[1,0,0,1],[1,0,0,2]]

**Output:** [1,0,1]

**Explanation:**

| `i` | `queries[i]`   | `nums`     | binary(`nums`) | popcount-<br>depth  | `[l, r]` | `k` | Valid<br>`nums[j]` | updated<br>`nums`  | Answer  |
|-----|----------------|------------|----------------|---------------------|----------|-----|--------------------|--------------------|---------|
| 0   | [1,0,1,1]      | [1, 2]     | [1, 10]        | [0, 1]              | [0, 1]   | 1   | [1]                | —                  | 1       |
| 1   | [2,0,3]        | [1, 2]     | [1, 10]        | [0, 1]              | —        | —   | —                  | [3, 2]             |         |
| 2   | [1,0,0,1]      | [3, 2]     | [11, 10]       | [2, 1]              | [0, 0]   | 1   | []                 | —                  | 0       |
| 3   | [1,0,0,2]      | [3, 2]     | [11, 10]       | [2, 1]              | [0, 0]   | 2   | [0]                | —                  | 1       |

Thus, the final `answer` is `[1, 0, 1]`.

**Constraints:**

*   <code>1 <= n == nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>15</sup></code>
*   <code>1 <= queries.length <= 10<sup>5</sup></code>
*   `queries[i].length == 3` or `4`
    *   `queries[i] == [1, l, r, k]` or,
    *   `queries[i] == [2, idx, val]`
    *   `0 <= l <= r <= n - 1`
    *   `0 <= k <= 5`
    *   `0 <= idx <= n - 1`
    *   <code>1 <= val <= 10<sup>15</sup></code>

## Solution

```kotlin
class Solution {
    private fun computeDepth(number: Long): Int {
        if (number == 1L) {
            return 0
        }
        return 1 + DEPTH_TABLE[java.lang.Long.bitCount(number)]
    }

    fun popcountDepth(nums: LongArray, queries: Array<LongArray>): IntArray {
        val len = nums.size
        val maxDepth = 6
        val trees = Array(maxDepth) { FenwickTree() }
        for (d in 0..<maxDepth) {
            trees[d].build(len)
        }
        for (i in 0..<len) {
            val depth = computeDepth(nums[i])
            if (depth < maxDepth) {
                trees[depth].update(i + 1, 1)
            }
        }
        val ansList = ArrayList<Int?>()
        for (query in queries) {
            val type = query[0].toInt()
            if (type == 1) {
                val left = query[1].toInt()
                val right = query[2].toInt()
                val depth = query[3].toInt()
                if (depth >= 0 && depth < maxDepth) {
                    ansList.add(trees[depth].queryRange(left + 1, right + 1))
                } else {
                    ansList.add(0)
                }
            } else if (type == 2) {
                val index = query[1].toInt()
                val newVal = query[2]
                val oldDepth = computeDepth(nums[index])
                if (oldDepth < maxDepth) {
                    trees[oldDepth].update(index + 1, -1)
                }
                nums[index] = newVal
                val newDepth = computeDepth(newVal)
                if (newDepth < maxDepth) {
                    trees[newDepth].update(index + 1, 1)
                }
            }
        }
        val ansArray = IntArray(ansList.size)
        for (i in ansList.indices) {
            ansArray[i] = ansList[i]!!
        }
        return ansArray
    }

    private class FenwickTree {
        private lateinit var tree: IntArray
        private var size = 0

        fun build(n: Int) {
            this.size = n
            this.tree = IntArray(size + 1)
        }

        fun update(index: Int, value: Int) {
            var index = index
            while (index <= size) {
                tree[index] += value
                index += index and (-index)
            }
        }

        fun query(index: Int): Int {
            var index = index
            var result = 0
            while (index > 0) {
                result += tree[index]
                index -= index and (-index)
            }
            return result
        }

        fun queryRange(left: Int, right: Int): Int {
            if (left > right) {
                return 0
            }
            return query(right) - query(left - 1)
        }
    }

    companion object {
        private val DEPTH_TABLE = IntArray(65)

        init {
            DEPTH_TABLE[1] = 0
            for (i in 2..64) {
                DEPTH_TABLE[i] = 1 + DEPTH_TABLE[Integer.bitCount(i)]
            }
        }
    }
}
```