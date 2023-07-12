[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2581\. Count Number of Possible Root Nodes

Hard

Alice has an undirected tree with `n` nodes labeled from `0` to `n - 1`. The tree is represented as a 2D integer array `edges` of length `n - 1` where <code>edges[i] = [a<sub>i</sub>, b<sub>i</sub>]</code> indicates that there is an edge between nodes <code>a<sub>i</sub></code> and <code>b<sub>i</sub></code> in the tree.

Alice wants Bob to find the root of the tree. She allows Bob to make several **guesses** about her tree. In one guess, he does the following:

*   Chooses two **distinct** integers `u` and `v` such that there exists an edge `[u, v]` in the tree.
*   He tells Alice that `u` is the **parent** of `v` in the tree.

Bob's guesses are represented by a 2D integer array `guesses` where <code>guesses[j] = [u<sub>j</sub>, v<sub>j</sub>]</code> indicates Bob guessed <code>u<sub>j</sub></code> to be the parent of <code>v<sub>j</sub></code>.

Alice being lazy, does not reply to each of Bob's guesses, but just says that **at least** `k` of his guesses are `true`.

Given the 2D integer arrays `edges`, `guesses` and the integer `k`, return _the **number of possible nodes** that can be the root of Alice's tree_. If there is no such tree, return `0`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2022/12/19/ex-1.png)

**Input:** edges = \[\[0,1],[1,2],[1,3],[4,2]], guesses = \[\[1,3],[0,1],[1,0],[2,4]], k = 3

**Output:** 3

**Explanation:** 

Root = 0, correct guesses = [1,3], [0,1], [2,4] 

Root = 1, correct guesses = [1,3], [1,0], [2,4] 

Root = 2, correct guesses = [1,3], [1,0], [2,4] 

Root = 3, correct guesses = [1,0], [2,4] 

Root = 4, correct guesses = [1,3], [1,0] 

Considering 0, 1, or 2 as root node leads to 3 correct guesses.

**Example 2:**

![](https://assets.leetcode.com/uploads/2022/12/19/ex-2.png)

**Input:** edges = \[\[0,1],[1,2],[2,3],[3,4]], guesses = \[\[1,0],[3,4],[2,1],[3,2]], k = 1

**Output:** 5

**Explanation:** 

Root = 0, correct guesses = [3,4] 

Root = 1, correct guesses = [1,0], [3,4] 

Root = 2, correct guesses = [1,0], [2,1], [3,4] 

Root = 3, correct guesses = [1,0], [2,1], [3,2], [3,4] 

Root = 4, correct guesses = [1,0], [2,1], [3,2] 

Considering any node as root will give at least 1 correct guess.

**Constraints:**

*   `edges.length == n - 1`
*   <code>2 <= n <= 10<sup>5</sup></code>
*   <code>1 <= guesses.length <= 10<sup>5</sup></code>
*   <code>0 <= a<sub>i</sub>, b<sub>i</sub>, u<sub>j</sub>, v<sub>j</sub> <= n - 1</code>
*   <code>a<sub>i</sub> != b<sub>i</sub></code>
*   <code>u<sub>j</sub> != v<sub>j</sub></code>
*   `edges` represents a valid tree.
*   `guesses[j]` is an edge of the tree.
*   `guesses` is unique.
*   `0 <= k <= guesses.length`

## Solution

```kotlin
import java.util.ArrayList
import java.util.HashSet

@Suppress("NAME_SHADOWING")
class Solution {
    private lateinit var parents: IntArray
    private lateinit var graph: Array<MutableList<Int>?>
    private lateinit var guess: Array<HashSet<Int>?>
    private var ans = 0

    fun rootCount(edges: Array<IntArray>, guesses: Array<IntArray>, k: Int): Int {
        val n = edges.size + 1
        graph = arrayOfNulls(n)
        guess = arrayOfNulls(n)
        for (i in 0 until n) {
            graph[i] = ArrayList()
            guess[i] = HashSet<Int>()
        }
        // Create tree
        for (i in edges.indices) {
            graph[edges[i][0]]!!.add(edges[i][1])
            graph[edges[i][1]]!!.add(edges[i][0])
        }
        // Create guess array
        for (i in guesses.indices) {
            guess[guesses[i][0]]!!.add(guesses[i][1])
        }
        parents = IntArray(n)
        fill(0, -1)
        var c = 0
        for (i in 1 until n) {
            if (guess[parents[i]]!!.contains(i)) c++
        }
        if (c >= k) ans++
        for (i in graph[0]!!) dfs(i, 0, c, k)
        return ans
    }

    // Fill the parent array
    private fun fill(v: Int, p: Int) {
        parents[v] = p
        for (child in graph[v]!!) {
            if (child == p) continue
            fill(child, v)
        }
    }

    // Use DFS to make all nodes as root one by one
    private fun dfs(v: Int, p: Int, c: Int, k: Int) {
        var c = c
        if (guess[p]!!.contains(v)) c--
        if (guess[v]!!.contains(p)) c++
        if (c >= k) ans++
        for (child in graph[v]!!) if (child != p) dfs(child, v, c, k)
    }
}
```