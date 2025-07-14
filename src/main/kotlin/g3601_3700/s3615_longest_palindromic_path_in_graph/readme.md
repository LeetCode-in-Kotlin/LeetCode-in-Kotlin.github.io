[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3615\. Longest Palindromic Path in Graph

Hard

You are given an integer `n` and an **undirected** graph with `n` nodes labeled from 0 to `n - 1` and a 2D array `edges`, where <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>]</code> indicates an edge between nodes <code>u<sub>i</sub></code> and <code>v<sub>i</sub></code>.

You are also given a string `label` of length `n`, where `label[i]` is the character associated with node `i`.

You may start at any node and move to any adjacent node, visiting each node **at most** once.

Return the **maximum** possible length of a **palindrome** that can be formed by visiting a set of **unique** nodes along a valid path.

**Example 1:**

**Input:** n = 3, edges = \[\[0,1],[1,2]], label = "aba"

**Output:** 3

**Exp****lanation:**

![](https://assets.leetcode.com/uploads/2025/06/13/screenshot-2025-06-13-at-230714.png)

*   The longest palindromic path is from node 0 to node 2 via node 1, following the path `0 → 1 → 2` forming string `"aba"`.
*   This is a valid palindrome of length 3.

**Example 2:**

**Input:** n = 3, edges = \[\[0,1],[0,2]], label = "abc"

**Output:** 1

**Explanation:**

![](https://assets.leetcode.com/uploads/2025/06/13/screenshot-2025-06-13-at-230017.png)

*   No path with more than one node forms a palindrome.
*   The best option is any single node, giving a palindrome of length 1.

**Example 3:**

**Input:** n = 4, edges = \[\[0,2],[0,3],[3,1]], label = "bbac"

**Output:** 3

**Explanation:**

![](https://assets.leetcode.com/uploads/2025/06/13/screenshot-2025-06-13-at-230508.png)

*   The longest palindromic path is from node 0 to node 1, following the path `0 → 3 → 1`, forming string `"bcb"`.
*   This is a valid palindrome of length 3.

**Constraints:**

*   `1 <= n <= 14`
*   `n - 1 <= edges.length <= n * (n - 1) / 2`
*   <code>edges[i] == [u<sub>i</sub>, v<sub>i</sub>]</code>
*   <code>0 <= u<sub>i</sub>, v<sub>i</sub> <= n - 1</code>
*   <code>u<sub>i</sub> != v<sub>i</sub></code>
*   `label.length == n`
*   `label` consists of lowercase English letters.
*   There are no duplicate edges.

## Solution

```kotlin
import kotlin.math.max

class Solution {
    fun maxLen(n: Int, edges: Array<IntArray>, labelsStr: String): Int {
        val labels = labelsStr.toCharArray()
        // collect lists of adjacent nodes
        val adj = adj(n, edges)
        // size of int to store n bits bitmask
        val bSize = 1 shl n
        val cache = Array<Array<IntArray>>(n) { Array<IntArray>(n) { IntArray(bSize) } }
        var maxLength = 0
        for (i in 0..<n) {
            // find palindromes of odd length (one node in the middle)
            val localLength = findPalindrome(adj, labels, i, i, 0, cache)
            maxLength = max(maxLength, localLength)
            // find palindromes of even length (two nodes in the middle)
            for (j in adj[i]) {
                if (labels[i] == labels[j]) {
                    val length = findPalindrome(adj, labels, i, j, 0, cache)
                    maxLength = max(maxLength, length)
                }
            }
        }
        return maxLength
    }

    private fun findPalindrome(
        adj: Array<IntArray>,
        labels: CharArray,
        i: Int,
        j: Int,
        b: Int,
        cache: Array<Array<IntArray>>,
    ): Int {
        if (cache[i][j][b] != 0) {
            return cache[i][j][b]
        }
        var b1 = set(b, i)
        b1 = set(b1, j)
        val length = if (i == j) 1 else 2
        var maxExtraLength = 0
        for (i1 in adj[i]) {
            if (get(b1, i1)) {
                continue
            }
            for (j1 in adj[j]) {
                if (i1 == j1) {
                    continue
                }
                if (labels[i1] != labels[j1]) {
                    continue
                }
                if (get(b1, j1)) {
                    continue
                }
                val extraLength = findPalindrome(adj, labels, i1, j1, b1, cache)
                maxExtraLength = max(maxExtraLength, extraLength)
            }
        }
        cache[i][j][b] = length + maxExtraLength
        return length + maxExtraLength
    }

    private fun get(b: Int, i: Int): Boolean {
        return (b and (1 shl i)) != 0
    }

    private fun set(b: Int, i: Int): Int {
        return b or (1 shl i)
    }

    private fun adj(n: Int, edges: Array<IntArray>): Array<IntArray> {
        val adjList: MutableList<MutableList<Int>> = ArrayList<MutableList<Int>>()
        for (i in 0..<n) {
            adjList.add(ArrayList<Int>())
        }
        for (edge in edges) {
            adjList[edge[0]].add(edge[1])
            adjList[edge[1]].add(edge[0])
        }
        val adj = Array<IntArray>(n) { i -> adjList[i].stream().mapToInt { j: Int -> j }.toArray() }
        return adj
    }
}
```