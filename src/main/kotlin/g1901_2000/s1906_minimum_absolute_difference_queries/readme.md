[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1906\. Minimum Absolute Difference Queries

Medium

The **minimum absolute difference** of an array `a` is defined as the **minimum value** of `|a[i] - a[j]|`, where `0 <= i < j < a.length` and `a[i] != a[j]`. If all elements of `a` are the **same**, the minimum absolute difference is `-1`.

*   For example, the minimum absolute difference of the array `[5,2,3,7,2]` is `|2 - 3| = 1`. Note that it is not `0` because `a[i]` and `a[j]` must be different.

You are given an integer array `nums` and the array `queries` where <code>queries[i] = [l<sub>i</sub>, r<sub>i</sub>]</code>. For each query `i`, compute the **minimum absolute difference** of the **subarray** <code>nums[l<sub>i</sub>...r<sub>i</sub>]</code> containing the elements of `nums` between the **0-based** indices <code>l<sub>i</sub></code> and <code>r<sub>i</sub></code> (**inclusive**).

Return _an **array**_ `ans` _where_ `ans[i]` _is the answer to the_ <code>i<sup>th</sup></code> _query_.

A **subarray** is a contiguous sequence of elements in an array.

The value of `|x|` is defined as:

*   `x` if `x >= 0`.
*   `-x` if `x < 0`.

**Example 1:**

**Input:** nums = [1,3,4,8], queries = \[\[0,1],[1,2],[2,3],[0,3]]

**Output:** [2,1,4,1]

**Explanation:** The queries are processed as follows: 

- queries[0] = [0,1]: The subarray is [1,3] and the minimum absolute difference is \|1-3\| = 2. 

- queries[1] = [1,2]: The subarray is [3,4] and the minimum absolute difference is \|3-4\| = 1. 

- queries[2] = [2,3]: The subarray is [4,8] and the minimum absolute difference is \|4-8\| = 4. 

- queries[3] = [0,3]: The subarray is [1,3,4,8] and the minimum absolute difference is \|3-4\| = 1.

**Example 2:**

**Input:** nums = [4,5,2,2,7,10], queries = \[\[2,3],[0,2],[0,5],[3,5]]

**Output:** [-1,1,1,3]

**Explanation:** The queries are processed as follows: 

- queries[0] = [2,3]: The subarray is [2,2] and the minimum absolute difference is -1 because all the elements are the same. 

- queries[1] = [0,2]: The subarray is [4,5,2] and the minimum absolute difference is \|4-5\| = 1. 

- queries[2] = [0,5]: The subarray is [4,5,2,2,7,10] and the minimum absolute difference is \|4-5\| = 1. 

- queries[3] = [3,5]: The subarray is [2,7,10] and the minimum absolute difference is \|7-10\| = 3.

**Constraints:**

*   <code>2 <= nums.length <= 10<sup>5</sup></code>
*   `1 <= nums[i] <= 100`
*   <code>1 <= queries.length <= 2 * 10<sup>4</sup></code>
*   <code>0 <= l<sub>i</sub> < r<sub>i</sub> < nums.length</code>

## Solution

```kotlin
import java.util.BitSet

class Solution {
    private class SegmentTree(nums: IntArray, len: Int) {
        class Node {
            var bits: BitSet? = null
            var minDiff = 0
        }

        var nums: IntArray
        var tree: Array<Node?>

        init {
            this.nums = nums.copyOf(len)
            tree = arrayOfNulls(4 * len)
            buildTree(0, len - 1, 0)
        }

        private fun buildTree(i: Int, j: Int, ti: Int) {
            if (i <= j) {
                if (i == j) {
                    val node = Node()
                    node.bits = BitSet(101)
                    node.bits!!.set(nums[i])
                    node.minDiff = INF
                    tree[ti] = node
                } else {
                    val mid = i + (j - i) / 2
                    buildTree(i, mid, 2 * ti + 1)
                    buildTree(mid + 1, j, 2 * ti + 2)
                    tree[ti] = combineNodes(tree[2 * ti + 1], tree[2 * ti + 2])
                }
            }
        }

        private fun combineNodes(n1: Node?, n2: Node?): Node {
            val node = Node()
            if (n1!!.minDiff == 1 || n2!!.minDiff == 1) {
                node.minDiff = 1
            } else {
                node.bits = BitSet(101)
                node.bits!!.or(n1.bits)
                node.bits!!.or(n2.bits)
                node.minDiff = findMinDiff(node.bits)
            }
            return node
        }

        private fun findMinDiff(bits: BitSet?): Int {
            // minimum value of number is 1.
            var first = bits!!.nextSetBit(1)
            var minDiff = INF
            while (first != -1) {
                val next = bits.nextSetBit(first + 1)
                if (next != -1) {
                    minDiff = Math.min(minDiff, next - first)
                    if (minDiff == 1) {
                        break
                    }
                }
                first = next
            }
            return minDiff
        }

        fun findMinAbsDiff(start: Int, end: Int, i: Int, j: Int, ti: Int): Int {
            val node = findMinAbsDiff2(start, end, i, j, ti)
            return if (node!!.minDiff == INF) -1 else node.minDiff
        }

        private fun findMinAbsDiff2(start: Int, end: Int, i: Int, j: Int, ti: Int): Node? {
            if (i == start && j == end) {
                return tree[ti]
            }
            val mid = i + (j - i) / 2
            return if (end <= mid) {
                findMinAbsDiff2(start, end, i, mid, 2 * ti + 1)
            } else if (start >= mid + 1) {
                findMinAbsDiff2(start, end, mid + 1, j, 2 * ti + 2)
            } else {
                val left = findMinAbsDiff2(start, mid, i, mid, 2 * ti + 1)
                val right = findMinAbsDiff2(mid + 1, end, mid + 1, j, 2 * ti + 2)
                combineNodes(left, right)
            }
        }

        companion object {
            const val INF = 200
        }
    }

    fun minDifference(nums: IntArray, queries: Array<IntArray>): IntArray {
        val len = nums.size
        val qlen = queries.size
        val st = SegmentTree(nums, len)
        val answer = IntArray(qlen)
        for (i in 0 until qlen) {
            answer[i] = st.findMinAbsDiff(queries[i][0], queries[i][1], 0, len - 1, 0)
        }
        return answer
    }
}
```