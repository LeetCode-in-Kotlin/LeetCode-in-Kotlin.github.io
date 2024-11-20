[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3165\. Maximum Sum of Subsequence With Non-adjacent Elements

Hard

You are given an array `nums` consisting of integers. You are also given a 2D array `queries`, where <code>queries[i] = [pos<sub>i</sub>, x<sub>i</sub>]</code>.

For query `i`, we first set <code>nums[pos<sub>i</sub>]</code> equal to <code>x<sub>i</sub></code>, then we calculate the answer to query `i` which is the **maximum** sum of a subsequence of `nums` where **no two adjacent elements are selected**.

Return the _sum_ of the answers to all queries.

Since the final answer may be very large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

A **subsequence** is an array that can be derived from another array by deleting some or no elements without changing the order of the remaining elements.

**Example 1:**

**Input:** nums = [3,5,9], queries = \[\[1,-2],[0,-3]]

**Output:** 21

**Explanation:**   
 After the 1<sup>st</sup> query, `nums = [3,-2,9]` and the maximum sum of a subsequence with non-adjacent elements is `3 + 9 = 12`.   
 After the 2<sup>nd</sup> query, `nums = [-3,-2,9]` and the maximum sum of a subsequence with non-adjacent elements is 9.

**Example 2:**

**Input:** nums = [0,-1], queries = \[\[0,-5]]

**Output:** 0

**Explanation:**   
 After the 1<sup>st</sup> query, `nums = [-5,-1]` and the maximum sum of a subsequence with non-adjacent elements is 0 (choosing an empty subsequence).

**Constraints:**

*   <code>1 <= nums.length <= 5 * 10<sup>4</sup></code>
*   <code>-10<sup>5</sup> <= nums[i] <= 10<sup>5</sup></code>
*   <code>1 <= queries.length <= 5 * 10<sup>4</sup></code>
*   <code>queries[i] == [pos<sub>i</sub>, x<sub>i</sub>]</code>
*   <code>0 <= pos<sub>i</sub> <= nums.length - 1</code>
*   <code>-10<sup>5</sup> <= x<sub>i</sub> <= 10<sup>5</sup></code>

## Solution

```kotlin
import kotlin.math.max

class Solution {
    fun maximumSumSubsequence(nums: IntArray, queries: Array<IntArray>): Int {
        val tree: Array<LongArray?> = build(nums)
        var result: Long = 0
        for (i in queries.indices) {
            result += set(tree, queries[i][0], queries[i][1])
            result %= MOD.toLong()
        }
        return result.toInt()
    }

    companion object {
        private const val YY = 0
        private const val YN = 1
        private const val NY = 2
        private const val NN = 3
        private const val MOD = 1000000007

        private fun build(nums: IntArray): Array<LongArray?> {
            val len = nums.size
            var size = 1
            while (size < len) {
                size = size shl 1
            }
            val tree = Array<LongArray?>(size * 2) { LongArray(4) }
            for (i in 0 until len) {
                tree[size + i]!![YY] = nums[i].toLong()
            }
            for (i in size - 1 downTo 1) {
                tree[i]!![YY] = max(
                    (tree[2 * i]!![YY] + tree[2 * i + 1]!![NY]),
                    (
                        tree[2 * i]!![YN] + max(
                            tree[2 * i + 1]!![YY],
                            tree[2 * i + 1]!![NY],
                        )
                        ),
                )
                tree[i]!![YN] = max(
                    (tree[2 * i]!![YY] + tree[2 * i + 1]!![NN]),
                    (
                        tree[2 * i]!![YN] + max(
                            tree[2 * i + 1]!![YN],
                            tree[2 * i + 1]!![NN],
                        )
                        ),
                )
                tree[i]!![NY] = max(
                    (tree[2 * i]!![NY] + tree[2 * i + 1]!![NY]),
                    (
                        tree[2 * i]!![NN] + max(
                            tree[2 * i + 1]!![YY],
                            tree[2 * i + 1]!![NY],
                        )
                        ),
                )
                tree[i]!![NN] = max(
                    (tree[2 * i]!![NY] + tree[2 * i + 1]!![NN]),
                    (
                        tree[2 * i]!![NN] + max(
                            tree[2 * i + 1]!![YN],
                            tree[2 * i + 1]!![NN],
                        )
                        ),
                )
            }
            return tree
        }

        private fun set(tree: Array<LongArray?>, idx: Int, `val`: Int): Long {
            val size = tree.size / 2
            tree[size + idx]!![YY] = `val`.toLong()
            var i = (size + idx) / 2
            while (i > 0) {
                tree[i]!![YY] = max(
                    (tree[2 * i]!![YY] + tree[2 * i + 1]!![NY]),
                    (
                        tree[2 * i]!![YN] + max(
                            tree[2 * i + 1]!![YY],
                            tree[2 * i + 1]!![NY],
                        )
                        ),
                )
                tree[i]!![YN] = max(
                    (tree[2 * i]!![YY] + tree[2 * i + 1]!![NN]),
                    (
                        tree[2 * i]!![YN] + max(
                            tree[2 * i + 1]!![YN],
                            tree[2 * i + 1]!![NN],
                        )
                        ),
                )
                tree[i]!![NY] = max(
                    (tree[2 * i]!![NY] + tree[2 * i + 1]!![NY]),
                    (
                        tree[2 * i]!![NN] + max(
                            tree[2 * i + 1]!![YY],
                            tree[2 * i + 1]!![NY],
                        )
                        ),
                )
                tree[i]!![NN] = max(
                    (tree[2 * i]!![NY] + tree[2 * i + 1]!![NN]),
                    (
                        tree[2 * i]!![NN] + max(
                            tree[2 * i + 1]!![YN],
                            tree[2 * i + 1]!![NN],
                        )
                        ),
                )
                i /= 2
            }
            return max(
                tree[1]!![YY],
                max(tree[1]!![YN], max(tree[1]!![NY], tree[1]!![NN])),
            )
        }
    }
}
```