[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1707\. Maximum XOR With an Element From Array

Hard

You are given an array `nums` consisting of non-negative integers. You are also given a `queries` array, where <code>queries[i] = [x<sub>i</sub>, m<sub>i</sub>]</code>.

The answer to the <code>i<sup>th</sup></code> query is the maximum bitwise `XOR` value of <code>x<sub>i</sub></code> and any element of `nums` that does not exceed <code>m<sub>i</sub></code>. In other words, the answer is <code>max(nums[j] XOR x<sub>i</sub>)</code> for all `j` such that <code>nums[j] <= m<sub>i</sub></code>. If all elements in `nums` are larger than <code>m<sub>i</sub></code>, then the answer is `-1`.

Return _an integer array_ `answer` _where_ `answer.length == queries.length` _and_ `answer[i]` _is the answer to the_ <code>i<sup>th</sup></code> _query._

**Example 1:**

**Input:** nums = [0,1,2,3,4], queries = \[\[3,1],[1,3],[5,6]]

**Output:** [3,3,7]

**Explanation:** 

1) 0 and 1 are the only two integers not greater than 1. 0 XOR 3 = 3 and 1 XOR 3 = 2. The larger of the two is 3. 

2) 1 XOR 2 = 3. 

3) 5 XOR 2 = 7.

**Example 2:**

**Input:** nums = [5,2,4,6,6,3], queries = \[\[12,4],[8,1],[6,3]]

**Output:** [15,-1,5]

**Constraints:**

*   <code>1 <= nums.length, queries.length <= 10<sup>5</sup></code>
*   `queries[i].length == 2`
*   <code>0 <= nums[j], x<sub>i</sub>, m<sub>i</sub> <= 10<sup>9</sup></code>

## Solution

```kotlin
@Suppress("NAME_SHADOWING")
class Solution {
    internal class QueryComparator : Comparator<IntArray> {
        override fun compare(a: IntArray, b: IntArray): Int {
            return a[1].compareTo(b[1])
        }
    }

    internal class Node {
        var zero: Node? = null
        var one: Node? = null
    }

    fun maximizeXor(nums: IntArray, queries: Array<IntArray>): IntArray {
        nums.sort()
        val len = queries.size
        val queryWithIndex = Array(len) { IntArray(3) }
        for (i in 0 until len) {
            queryWithIndex[i][0] = queries[i][0]
            queryWithIndex[i][1] = queries[i][1]
            queryWithIndex[i][2] = i
        }
        queryWithIndex.sortWith(QueryComparator())
        var numId = 0
        val ans = IntArray(len)
        val root = Node()
        for (i in 0 until len) {
            while (numId < nums.size && nums[numId] <= queryWithIndex[i][1]) {
                addNumToTree(nums[numId], root)
                numId++
            }
            ans[queryWithIndex[i][2]] = maxXOR(queryWithIndex[i][0], root)
        }
        return ans
    }

    private fun addNumToTree(num: Int, node: Node) {
        var node: Node? = node
        for (i in 31 downTo 0) {
            val digit = num shr i and 1
            if (digit == 1) {
                if (node!!.one == null) {
                    node.one = Node()
                }
                node = node.one
            } else {
                if (node!!.zero == null) {
                    node.zero = Node()
                }
                node = node.zero
            }
        }
    }

    private fun maxXOR(num: Int, node: Node): Int {
        var node: Node? = node
        if (node!!.one == null && node.zero == null) {
            return -1
        }
        var ans = 0
        var i = 31
        while (i >= 0 && node != null) {
            val digit = num shr i and 1
            if (digit == 1) {
                if (node.zero != null) {
                    ans += 1 shl i
                    node = node.zero
                } else {
                    node = node.one
                }
            } else {
                if (node.one != null) {
                    ans += 1 shl i
                    node = node.one
                } else {
                    node = node.zero
                }
            }
            i--
        }
        return ans
    }
}
```