[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3820\. Pythagorean Distance Nodes in a Tree

Medium

You are given an integer `n` and an undirected tree with `n` nodes numbered from 0 to `n - 1`. The tree is represented by a 2D array `edges` of length `n - 1`, where <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>]</code> indicates an undirected edge between <code>u<sub>i</sub></code> and <code>v<sub>i</sub></code>.

You are also given three **distinct** target nodes `x`, `y`, and `z`.

For any node `u` in the tree:

*   Let `dx` be the distance from `u` to node `x`
*   Let `dy` be the distance from `u` to node `y`
*   Let `dz` be the distance from `u` to node `z`

The node `u` is called **special** if the three distances form a **Pythagorean Triplet**.

Return an integer denoting the number of special nodes in the tree.

A **Pythagorean triplet** consists of three integers `a`, `b`, and `c` which, when sorted in **ascending** order, satisfy <code>a<sup>2</sup> + b<sup>2</sup> = c<sup>2</sup></code>.

The **distance** between two nodes in a tree is the number of edges on the unique path between them.

**Example 1:**

**Input:** n = 4, edges = \[\[0,1],[0,2],[0,3]], x = 1, y = 2, z = 3

**Output:** 3

**Explanation:**

For each node, we compute its distances to nodes `x = 1`, `y = 2`, and `z = 3`.

*   Node 0 has distances 1, 1, and 1. After sorting, the distances are 1, 1, and 1, which do not satisfy the Pythagorean condition.
*   Node 1 has distances 0, 2, and 2. After sorting, the distances are 0, 2, and 2. Since <code>0<sup>2</sup> + 2<sup>2</sup> = 2<sup>2</sup></code>, node 1 is special.
*   Node 2 has distances 2, 0, and 2. After sorting, the distances are 0, 2, and 2. Since <code>0<sup>2</sup> + 2<sup>2</sup> = 2<sup>2</sup></code>, node 2 is special.
*   Node 3 has distances 2, 2, and 0. After sorting, the distances are 0, 2, and 2. This also satisfies the Pythagorean condition.

Therefore, nodes 1, 2, and 3 are special, and the answer is 3.

**Example 2:**

**Input:** n = 4, edges = \[\[0,1],[1,2],[2,3]], x = 0, y = 3, z = 2

**Output:** 0

**Explanation:**

For each node, we compute its distances to nodes `x = 0`, `y = 3`, and `z = 2`.

*   Node 0 has distances 0, 3, and 2. After sorting, the distances are 0, 2, and 3, which do not satisfy the Pythagorean condition.
*   Node 1 has distances 1, 2, and 1. After sorting, the distances are 1, 1, and 2, which do not satisfy the Pythagorean condition.
*   Node 2 has distances 2, 1, and 0. After sorting, the distances are 0, 1, and 2, which do not satisfy the Pythagorean condition.
*   Node 3 has distances 3, 0, and 1. After sorting, the distances are 0, 1, and 3, which do not satisfy the Pythagorean condition.

No node satisfies the Pythagorean condition. Therefore, the answer is 0.

**Example 3:**

**Input:** n = 4, edges = \[\[0,1],[1,2],[1,3]], x = 1, y = 3, z = 0

**Output:** 1

**Explanation:**

For each node, we compute its distances to nodes `x = 1`, `y = 3`, and `z = 0`.

*   Node 0 has distances 1, 2, and 0. After sorting, the distances are 0, 1, and 2, which do not satisfy the Pythagorean condition.
*   Node 1 has distances 0, 1, and 1. After sorting, the distances are 0, 1, and 1. Since <code>0<sup>2</sup> + 1<sup>2</sup> = 1<sup>2</sup></code>, node 1 is special.
*   Node 2 has distances 1, 2, and 2. After sorting, the distances are 1, 2, and 2, which do not satisfy the Pythagorean condition.
*   Node 3 has distances 1, 0, and 2. After sorting, the distances are 0, 1, and 2, which do not satisfy the Pythagorean condition.

Therefore, the answer is 1.

**Constraints:**

*   <code>4 <= n <= 10<sup>5</sup></code>
*   `edges.length == n - 1`
*   <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>]</code>
*   <code>0 <= u<sub>i</sub>, v<sub>i</sub>, x, y, z <= n - 1</code>
*   `x`, `y`, and `z` are pairwise **distinct**.
*   The input is generated such that `edges` represent a valid tree.

## Solution

```kotlin
class Solution {
    fun bfs(n: Int, adj: Array<MutableList<Int>>, start: Int): IntArray {
        val dist = IntArray(n)
        dist.fill(-1)
        dist[start] = 0
        val q = java.util.ArrayDeque<Int>()
        q.add(start)
        while (q.isNotEmpty()) {
            val u: Int = q.poll()!!
            for (v in adj[u]) {
                // Check if this neighbour was not visited yet
                if (dist[v] == -1) {
                    dist[v] = dist[u] + 1
                    q.add(v)
                }
            }
        }
        return dist
    }

    fun specialNodes(n: Int, edges: Array<IntArray>, x: Int, y: Int, z: Int): Int {
        val adj: Array<MutableList<Int>> = Array<MutableList<Int>>(n) { ArrayList() }
        for (i in 0..<n) {
            adj[i] = ArrayList<Int>()
        }
        for (edge in edges) {
            val u = edge[0]
            val v = edge[1]
            adj[u].add(v)
            adj[v].add(u)
        }
        // Calculate distances from every node to x, y and z
        val dx = bfs(n, adj, x)
        val dy = bfs(n, adj, y)
        val dz = bfs(n, adj, z)
        var result = 0
        for (i in 0..<n) {
            var a = dx[i].toLong()
            var b = dy[i]
            var c = dz[i]
            // Ensure a <= b <= c
            if (a > b) {
                val t = a
                a = b.toLong()
                b = t.toInt()
            }
            if (b > c) {
                val t = b.toLong()
                b = c
                c = t.toInt()
            }
            if (a > b) {
                val t = a
                a = b.toLong()
                b = t.toInt()
            }
            result += if (a * a + b.toLong() * b == c.toLong() * c) 1 else 0
        }
        return result
    }
}
```