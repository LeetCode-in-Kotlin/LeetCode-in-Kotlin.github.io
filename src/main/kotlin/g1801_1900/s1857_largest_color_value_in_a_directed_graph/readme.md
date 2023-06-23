[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1857\. Largest Color Value in a Directed Graph

Hard

There is a **directed graph** of `n` colored nodes and `m` edges. The nodes are numbered from `0` to `n - 1`.

You are given a string `colors` where `colors[i]` is a lowercase English letter representing the **color** of the <code>i<sup>th</sup></code> node in this graph (**0-indexed**). You are also given a 2D array `edges` where <code>edges[j] = [a<sub>j</sub>, b<sub>j</sub>]</code> indicates that there is a **directed edge** from node <code>a<sub>j</sub></code> to node <code>b<sub>j</sub></code>.

A valid **path** in the graph is a sequence of nodes <code>x<sub>1</sub> -> x<sub>2</sub> -> x<sub>3</sub> -> ... -> x<sub>k</sub></code> such that there is a directed edge from <code>x<sub>i</sub></code> to <code>x<sub>i+1</sub></code> for every `1 <= i < k`. The **color value** of the path is the number of nodes that are colored the **most frequently** occurring color along that path.

Return _the **largest color value** of any valid path in the given graph, or_ `-1` _if the graph contains a cycle_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/04/21/leet1.png)

**Input:** colors = "abaca", edges = \[\[0,1],[0,2],[2,3],[3,4]]

**Output:** 3

**Explanation:** The path 0 -> 2 -> 3 -> 4 contains 3 nodes that are colored `"a" (red in the above image)`.

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/04/21/leet2.png)

**Input:** colors = "a", edges = \[\[0,0]]

**Output:** -1

**Explanation:** There is a cycle from 0 to 0.

**Constraints:**

*   `n == colors.length`
*   `m == edges.length`
*   <code>1 <= n <= 10<sup>5</sup></code>
*   <code>0 <= m <= 10<sup>5</sup></code>
*   `colors` consists of lowercase English letters.
*   <code>0 <= a<sub>j</sub>, b<sub>j</sub> < n</code>

## Solution

```kotlin
class Solution {
    fun largestPathValue(colors: String, edges: Array<IntArray>): Int {
        val len = colors.length
        val graph = buildGraph(len, edges)
        val frequencies = IntArray(26)
        val calculatedFrequencies = HashMap<Int, IntArray>()
        val status = IntArray(len)
        for (i in 0 until len) {
            if (status[i] != 0) {
                continue
            }
            val localMax = runDFS(graph, i, calculatedFrequencies, status, colors)
            if (localMax!![26] == -1) {
                frequencies.fill(-1)
                break
            } else {
                for (color in 0..25) {
                    frequencies[color] = Math.max(frequencies[color], localMax[color])
                }
            }
        }
        var max = Int.MIN_VALUE
        for (freq in frequencies) {
            max = Math.max(max, freq)
        }
        return max
    }

    private fun runDFS(
        graph: Array<MutableList<Int>?>,
        node: Int,
        calculatedFrequencies: HashMap<Int, IntArray>,
        status: IntArray,
        colors: String
    ): IntArray? {
        if (calculatedFrequencies.containsKey(node)) {
            return calculatedFrequencies[node]
        }
        val frequencies = IntArray(27)
        if (status[node] == 1) {
            frequencies[26] = -1
            return frequencies
        }
        status[node] = 1
        for (neighbour in graph[node]!!) {
            val localMax = runDFS(graph, neighbour, calculatedFrequencies, status, colors)
            if (localMax!![26] == -1) {
                return localMax
            }
            for (i in 0..25) {
                frequencies[i] = Math.max(frequencies[i], localMax[i])
            }
        }
        status[node] = 2
        val color = colors[node].code - 'a'.code
        frequencies[color]++
        calculatedFrequencies[node] = frequencies
        return frequencies
    }

    private fun buildGraph(n: Int, edges: Array<IntArray>): Array<MutableList<Int>?> {
        val graph: Array<MutableList<Int>?> = arrayOfNulls(n)
        for (i in 0 until n) {
            graph[i] = ArrayList()
        }
        for (edge in edges) {
            graph[edge[0]]?.add(edge[1])
        }
        return graph
    }
}
```