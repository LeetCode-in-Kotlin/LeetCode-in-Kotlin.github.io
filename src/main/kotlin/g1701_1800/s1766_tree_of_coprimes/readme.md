[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1766\. Tree of Coprimes

Hard

There is a tree (i.e., a connected, undirected graph that has no cycles) consisting of `n` nodes numbered from `0` to `n - 1` and exactly `n - 1` edges. Each node has a value associated with it, and the **root** of the tree is node `0`.

To represent this tree, you are given an integer array `nums` and a 2D array `edges`. Each `nums[i]` represents the <code>i<sup>th</sup></code> node's value, and each <code>edges[j] = [u<sub>j</sub>, v<sub>j</sub>]</code> represents an edge between nodes <code>u<sub>j</sub></code> and <code>v<sub>j</sub></code> in the tree.

Two values `x` and `y` are **coprime** if `gcd(x, y) == 1` where `gcd(x, y)` is the **greatest common divisor** of `x` and `y`.

An ancestor of a node `i` is any other node on the shortest path from node `i` to the **root**. A node is **not** considered an ancestor of itself.

Return _an array_ `ans` _of size_ `n`, _where_ `ans[i]` _is the closest ancestor to node_ `i` _such that_ `nums[i]` _and_ `nums[ans[i]]` are **coprime**, or `-1` _if there is no such ancestor_.

**Example 1:**

**![](https://assets.leetcode.com/uploads/2021/01/06/untitled-diagram.png)**

**Input:** nums = [2,3,3,2], edges = \[\[0,1],[1,2],[1,3]]

**Output:** [-1,0,0,1]

**Explanation:** In the above figure, each node's value is in parentheses. 

- Node 0 has no coprime ancestors. 

- Node 1 has only one ancestor, node 0. Their values are coprime (gcd(2,3) == 1). - Node 2 has two ancestors, nodes 1 and 0. Node 1's value is not coprime (gcd(3,3) == 3), but node 0's value is (gcd(2,3) == 1), so node 0 is the closest valid ancestor. 

- Node 3 has two ancestors, nodes 1 and 0. It is coprime with node 1 (gcd(3,2) == 1), so node 1 is its closest valid ancestor.

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/01/06/untitled-diagram1.png)

**Input:** nums = [5,6,10,2,3,6,15], edges = \[\[0,1],[0,2],[1,3],[1,4],[2,5],[2,6]]

**Output:** [-1,0,-1,0,0,0,-1]

**Constraints:**

*   `nums.length == n`
*   `1 <= nums[i] <= 50`
*   <code>1 <= n <= 10<sup>5</sup></code>
*   `edges.length == n - 1`
*   `edges[j].length == 2`
*   <code>0 <= u<sub>j</sub>, v<sub>j</sub> < n</code>
*   <code>u<sub>j</sub> != v<sub>j</sub></code>

## Solution

```kotlin
@Suppress("kotlin:S107")
class Solution {
    private fun dfs(
        v2n: IntArray,
        v2d: IntArray,
        depth: Int,
        parent: Int,
        node: Int,
        ans: IntArray,
        nums: IntArray,
        neighbors: Array<ArrayList<Int>>
    ) {
        var d = Int.MIN_VALUE
        var n = -1
        val v = nums[node]
        for (i in 1..50) {
            if (v2n[i] != -1 && v2d[i] > d && gcd(i, v) == 1) {
                d = v2d[i]
                n = v2n[i]
            }
        }
        ans[node] = n
        val v2NOld = v2n[v]
        val v2DOld = v2d[v]
        v2n[v] = node
        v2d[v] = depth
        for (child in neighbors[node]) {
            if (child == parent) {
                continue
            }
            dfs(v2n, v2d, depth + 1, node, child, ans, nums, neighbors)
        }
        v2n[v] = v2NOld
        v2d[v] = v2DOld
    }

    private fun gcd(x: Int, y: Int): Int {
        return if (x == 0) y else gcd(y % x, x)
    }

    fun getCoprimes(nums: IntArray, edges: Array<IntArray>): IntArray {
        val n = nums.size
        val neighbors: Array<ArrayList<Int>> = Array(n) { ArrayList() }
        for (i in 0 until n) {
            neighbors[i] = ArrayList()
        }
        for (edge in edges) {
            neighbors[edge[0]].add(edge[1])
            neighbors[edge[1]].add(edge[0])
        }
        val ans = IntArray(n)
        val v2n = IntArray(51)
        val v2d = IntArray(51)
        v2n.fill(-1)
        dfs(v2n, v2d, 0, -1, 0, ans, nums, neighbors)
        return ans
    }
}
```