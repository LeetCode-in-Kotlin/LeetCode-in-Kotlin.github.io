[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1439\. Find the Kth Smallest Sum of a Matrix With Sorted Rows

Hard

You are given an `m x n` matrix `mat` that has its rows sorted in non-decreasing order and an integer `k`.

You are allowed to choose **exactly one element** from each row to form an array.

Return _the_ <code>k<sup>th</sup></code> _smallest array sum among all possible arrays_.

**Example 1:**

**Input:** mat = \[\[1,3,11],[2,4,6]], k = 5

**Output:** 7

**Explanation:** Choosing one element from each row, the first k smallest sum are: [1,2], [1,4], [3,2], [3,4], [1,6]. Where the 5th sum is 7.

**Example 2:**

**Input:** mat = \[\[1,3,11],[2,4,6]], k = 9

**Output:** 17

**Example 3:**

**Input:** mat = \[\[1,10,10],[1,4,5],[2,3,6]], k = 7

**Output:** 9

**Explanation:** Choosing one element from each row, the first k smallest sum are: [1,1,2], [1,1,3], [1,4,2], [1,4,3], [1,1,6], [1,5,2], [1,5,3]. Where the 7th sum is 9.

**Constraints:**

*   `m == mat.length`
*   `n == mat.length[i]`
*   `1 <= m, n <= 40`
*   `1 <= mat[i][j] <= 5000`
*   <code>1 <= k <= min(200, n<sup>m</sup>)</code>
*   `mat[i]` is a non-decreasing array.

## Solution

```kotlin
import java.util.Objects
import java.util.TreeSet

@Suppress("kotlin:S6510")
class Solution {
    fun kthSmallest(mat: Array<IntArray>, k: Int): Int {
        val treeSet = TreeSet(
            Comparator { o1: IntArray, o2: IntArray ->
                if (o1[0] != o2[0]) {
                    return@Comparator o1[0] - o2[0]
                } else {
                    for (i in 1 until o1.size) {
                        if (o1[i] != o2[i]) {
                            return@Comparator o1[i] - o2[i]
                        }
                    }
                    return@Comparator 0
                }
            }
        )
        val m = mat.size
        val n = mat[0].size
        var sum = 0
        val entry = IntArray(m + 1)
        for (ints in mat) {
            sum += ints[0]
        }
        entry[0] = sum
        treeSet.add(entry)
        var count = 0
        while (count < k) {
            val curr: IntArray = treeSet.pollFirst() as IntArray
            count++
            if (count == k) {
                return Objects.requireNonNull(curr)[0]
            }
            for (i in 0 until m) {
                val next = Objects.requireNonNull(curr).copyOf(curr.size)
                if (curr[i + 1] + 1 < n) {
                    next[0] -= mat[i][curr[i + 1]]
                    next[0] += mat[i][curr[i + 1] + 1]
                    next[i + 1]++
                    treeSet.add(next)
                }
            }
        }
        return -1
    }
}
```