[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 886\. Possible Bipartition

Medium

We want to split a group of `n` people (labeled from `1` to `n`) into two groups of **any size**. Each person may dislike some other people, and they should not go into the same group.

Given the integer `n` and the array `dislikes` where <code>dislikes[i] = [a<sub>i</sub>, b<sub>i</sub>]</code> indicates that the person labeled <code>a<sub>i</sub></code> does not like the person labeled <code>b<sub>i</sub></code>, return `true` _if it is possible to split everyone into two groups in this way_.

**Example 1:**

**Input:** n = 4, dislikes = \[\[1,2],[1,3],[2,4]]

**Output:** true

**Explanation:** The first group has [1,4], and the second group has [2,3].

**Example 2:**

**Input:** n = 3, dislikes = \[\[1,2],[1,3],[2,3]]

**Output:** false

**Explanation:** We need at least 3 groups to divide them. We cannot put them in two groups.

**Constraints:**

*   `1 <= n <= 2000`
*   <code>0 <= dislikes.length <= 10<sup>4</sup></code>
*   `dislikes[i].length == 2`
*   <code>1 <= a<sub>i</sub> < b<sub>i</sub> <= n</code>
*   All the pairs of `dislikes` are **unique**.

## Solution

```kotlin
class Solution {
    fun possibleBipartition(n: Int, dislikes: Array<IntArray>): Boolean {
        // build graph
        val g = Graph(n)
        for (dislike in dislikes) {
            g.addEdge(dislike[0] - 1, dislike[1] - 1)
        }
        val marked = BooleanArray(n)
        val colors = BooleanArray(n)
        for (v in 0 until n) {
            if (!marked[v] && !checkBipartiteDFS(g, marked, colors, v)) {
                // No need to run on other connected components if one component has failed.
                return false
            }
        }
        return true
    }

    private fun checkBipartiteDFS(g: Graph, marked: BooleanArray, colors: BooleanArray, v: Int): Boolean {
        marked[v] = true
        for (w in g.adj(v)) {
            if (!marked[w]) {
                colors[w] = !colors[v]
                if (!checkBipartiteDFS(g, marked, colors, w)) {
                    // this is to break for other neighbours
                    return false
                }
            } else if (colors[v] == colors[w]) {
                return false
            }
        }
        return true
    }

    private class Graph(v: Int) {
        private val adj: Array<ArrayList<Int>?>

        init {
            adj = arrayOfNulls(v)
            for (i in 0 until v) {
                adj[i] = ArrayList<Int>()
            }
        }

        fun addEdge(v: Int, w: Int) {
            adj[v]!!.add(w)
            adj[w]!!.add(v)
        }

        fun adj(v: Int): List<Int> {
            return adj[v]!!
        }
    }
}
```