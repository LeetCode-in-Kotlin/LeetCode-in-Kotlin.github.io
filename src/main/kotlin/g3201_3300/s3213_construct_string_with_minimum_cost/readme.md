[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3213\. Construct String with Minimum Cost

Hard

You are given a string `target`, an array of strings `words`, and an integer array `costs`, both arrays of the same length.

Imagine an empty string `s`.

You can perform the following operation any number of times (including **zero**):

*   Choose an index `i` in the range `[0, words.length - 1]`.
*   Append `words[i]` to `s`.
*   The cost of operation is `costs[i]`.

Return the **minimum** cost to make `s` equal to `target`. If it's not possible, return `-1`.

**Example 1:**

**Input:** target = "abcdef", words = ["abdef","abc","d","def","ef"], costs = [100,1,1,10,5]

**Output:** 7

**Explanation:**

The minimum cost can be achieved by performing the following operations:

*   Select index 1 and append `"abc"` to `s` at a cost of 1, resulting in `s = "abc"`.
*   Select index 2 and append `"d"` to `s` at a cost of 1, resulting in `s = "abcd"`.
*   Select index 4 and append `"ef"` to `s` at a cost of 5, resulting in `s = "abcdef"`.

**Example 2:**

**Input:** target = "aaaa", words = ["z","zz","zzz"], costs = [1,10,100]

**Output:** \-1

**Explanation:**

It is impossible to make `s` equal to `target`, so we return -1.

**Constraints:**

*   <code>1 <= target.length <= 5 * 10<sup>4</sup></code>
*   <code>1 <= words.length == costs.length <= 5 * 10<sup>4</sup></code>
*   `1 <= words[i].length <= target.length`
*   The total sum of `words[i].length` is less than or equal to <code>5 * 10<sup>4</sup></code>.
*   `target` and `words[i]` consist only of lowercase English letters.
*   <code>1 <= costs[i] <= 10<sup>4</sup></code>

## Solution

```kotlin
import kotlin.math.min

class Solution {
    private class ACAutomaton {
        class Node {
            var key: Char = 0.toChar()
            var `val`: Int? = null
            var len: Int = 0
            val next: Array<Node?> = arrayOfNulls(26)
            var suffix: Node? = null
            var output: Node? = null
            var parent: Node? = null
        }

        fun build(patterns: Array<String>, values: IntArray): Node {
            val root = Node()
            root.suffix = root
            root.output = root
            for (i in patterns.indices) {
                put(root, patterns[i], values[i])
            }
            for (i in root.next.indices) {
                if (root.next[i] == null) {
                    root.next[i] = root
                } else {
                    root.next[i]!!.suffix = root
                }
            }
            return root
        }

        private fun put(root: Node, s: String, `val`: Int) {
            var node: Node? = root
            for (c in s.toCharArray()) {
                if (node!!.next[c.code - 'a'.code] == null) {
                    node.next[c.code - 'a'.code] = Node()
                    node.next[c.code - 'a'.code]!!.parent = node
                    node.next[c.code - 'a'.code]!!.key = c
                }
                node = node.next[c.code - 'a'.code]
            }
            if (node!!.`val` == null || node.`val`!! > `val`) {
                node.`val` = `val`
                node.len = s.length
            }
        }

        fun getOutput(node: Node?): Node? {
            if (node!!.output == null) {
                val suffix = getSuffix(node)
                node.output = if (suffix!!.`val` != null) suffix else getOutput(suffix)
            }
            return node.output
        }

        fun go(node: Node?, c: Char): Node? {
            if (node!!.next[c.code - 'a'.code] == null) {
                node.next[c.code - 'a'.code] = go(getSuffix(node), c)
            }
            return node.next[c.code - 'a'.code]
        }

        private fun getSuffix(node: Node?): Node? {
            if (node!!.suffix == null) {
                node.suffix = go(getSuffix(node.parent), node.key)
                if (node.suffix!!.`val` != null) {
                    node.output = node.suffix
                } else {
                    node.output = node.suffix!!.output
                }
            }
            return node.suffix
        }
    }

    fun minimumCost(target: String, words: Array<String>, costs: IntArray): Int {
        val ac = ACAutomaton()
        val root = ac.build(words, costs)
        val dp = IntArray(target.length + 1)
        dp.fill(Int.MAX_VALUE / 2)
        dp[0] = 0
        var node: ACAutomaton.Node? = root
        for (i in 1 until dp.size) {
            node = ac.go(node, target[i - 1])
            var temp = node
            while (temp != null && temp !== root) {
                if (temp.`val` != null && dp[i - temp.len] < Int.MAX_VALUE / 2) {
                    dp[i] = min(dp[i], (dp[i - temp.len] + temp.`val`!!))
                }
                temp = ac.getOutput(temp)
            }
        }
        return if (dp[dp.size - 1] >= Int.MAX_VALUE / 2) -1 else dp[dp.size - 1]
    }
}
```