[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3161\. Block Placement Queries

Hard

There exists an infinite number line, with its origin at 0 and extending towards the **positive** x-axis.

You are given a 2D array `queries`, which contains two types of queries:

1.  For a query of type 1, `queries[i] = [1, x]`. Build an obstacle at distance `x` from the origin. It is guaranteed that there is **no** obstacle at distance `x` when the query is asked.
2.  For a query of type 2, `queries[i] = [2, x, sz]`. Check if it is possible to place a block of size `sz` _anywhere_ in the range `[0, x]` on the line, such that the block **entirely** lies in the range `[0, x]`. A block **cannot** be placed if it intersects with any obstacle, but it may touch it. Note that you do **not** actually place the block. Queries are separate.

Return a boolean array `results`, where `results[i]` is `true` if you can place the block specified in the <code>i<sup>th</sup></code> query of type 2, and `false` otherwise.

**Example 1:**

**Input:** queries = \[\[1,2],[2,3,3],[2,3,1],[2,2,2]]

**Output:** [false,true,true]

**Explanation:**

**![](https://assets.leetcode.com/uploads/2024/04/22/example0block.png)**

For query 0, place an obstacle at `x = 2`. A block of size at most 2 can be placed before `x = 3`.

**Example 2:**

**Input:** queries = \[\[1,7],[2,7,6],[1,2],[2,7,5],[2,7,6]]

**Output:** [true,true,false]

**Explanation:**

**![](https://assets.leetcode.com/uploads/2024/04/22/example1block.png)**

*   Place an obstacle at `x = 7` for query 0. A block of size at most 7 can be placed before `x = 7`.
*   Place an obstacle at `x = 2` for query 2. Now, a block of size at most 5 can be placed before `x = 7`, and a block of size at most 2 before `x = 2`.

**Constraints:**

*   <code>1 <= queries.length <= 15 * 10<sup>4</sup></code>
*   `2 <= queries[i].length <= 3`
*   `1 <= queries[i][0] <= 2`
*   <code>1 <= x, sz <= min(5 * 10<sup>4</sup>, 3 * queries.length)</code>
*   The input is generated such that for queries of type 1, no obstacle exists at distance `x` when the query is asked.
*   The input is generated such that there is at least one query of type 2.

## Solution

```kotlin
import kotlin.math.max

class Solution {
    private class Seg private constructor(private val start: Int, private val end: Int) {
        private var min = 0
        private var max = 0
        private var len = 0
        private var obstacle = false
        private lateinit var left: Seg
        private lateinit var right: Seg

        init {
            if (start < end) {
                val mid = start + ((end - start) shr 1)
                left = Seg(start, mid)
                right = Seg(mid + 1, end)
                refresh()
            }
        }

        fun set(i: Int) {
            if (i < start || i > end) {
                return
            } else if (i == start && i == end) {
                obstacle = true
                max = start
                min = max
                return
            }
            left.set(i)
            right.set(i)
            refresh()
        }

        private fun refresh() {
            if (left.obstacle) {
                min = left.min
                if (right.obstacle) {
                    max = right.max
                    len = max((right.min - left.max), max(left.len, right.len))
                } else {
                    max = left.max
                    len = max(left.len, (right.end - left.max))
                }
                obstacle = true
            } else if (right.obstacle) {
                min = right.min
                max = right.max
                len = max(right.len, (right.min - left.start))
                obstacle = true
            } else {
                len = end - start
            }
        }

        fun max(n: Int, t: IntArray) {
            if (end <= n) {
                t[0] = max(t[0], len)
                if (obstacle) {
                    t[1] = max
                }
                return
            }
            left.max(n, t)
            if (!right.obstacle || right.min >= n) {
                return
            }
            t[0] = max(t[0], (right.min - t[1]))
            right.max(n, t)
        }

        companion object {
            fun init(n: Int): Seg {
                return Seg(0, n)
            }
        }
    }

    fun getResults(queries: Array<IntArray>): List<Boolean> {
        var max = 0
        for (i in queries) {
            max = max(max, i[1])
        }
        val root = Seg.init(max)
        root.set(0)

        val res: MutableList<Boolean> = ArrayList(queries.size)
        for (i in queries) {
            if (i[0] == 1) {
                root.set(i[1])
            } else {
                val t = IntArray(2)
                root.max(i[1], t)
                res.add(max(t[0], (i[1] - t[1])) >= i[2])
            }
        }
        return res
    }
}
```