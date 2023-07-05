[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2538\. Difference Between Maximum and Minimum Price Sum

Hard

There exists an undirected and initially unrooted tree with `n` nodes indexed from `0` to `n - 1`. You are given the integer `n` and a 2D integer array `edges` of length `n - 1`, where <code>edges[i] = [a<sub>i</sub>, b<sub>i</sub>]</code> indicates that there is an edge between nodes <code>a<sub>i</sub></code> and <code>b<sub>i</sub></code> in the tree.

Each node has an associated price. You are given an integer array `price`, where `price[i]` is the price of the <code>i<sup>th</sup></code> node.

The **price sum** of a given path is the sum of the prices of all nodes lying on that path.

The tree can be rooted at any node `root` of your choice. The incurred **cost** after choosing `root` is the difference between the maximum and minimum **price sum** amongst all paths starting at `root`.

Return _the **maximum** possible **cost**_ _amongst all possible root choices_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2022/12/01/example14.png)

**Input:** n = 6, edges = \[\[0,1],[1,2],[1,3],[3,4],[3,5]], price = [9,8,7,6,10,5]

**Output:** 24

**Explanation:** The diagram above denotes the tree after rooting it at node 2. The first part (colored in red) shows the path with the maximum price sum. The second part (colored in blue) shows the path with the minimum price sum. 

- The first path contains nodes [2,1,3,4]: the prices are [7,8,6,10], and the sum of the prices is 31. 

- The second path contains the node [2] with the price [7]. 

The difference between the maximum and minimum price sum is 24. It can be proved that 24 is the maximum cost.

**Example 2:**

![](https://assets.leetcode.com/uploads/2022/11/24/p1_example2.png)

**Input:** n = 3, edges = \[\[0,1],[1,2]], price = [1,1,1]

**Output:** 2

**Explanation:** The diagram above denotes the tree after rooting it at node 0. The first part (colored in red) shows the path with the maximum price sum. The second part (colored in blue) shows the path with the minimum price sum. 

- The first path contains nodes [0,1,2]: the prices are [1,1,1], and the sum of the prices is 3.

- The second path contains node [0] with a price [1]. 

The difference between the maximum and minimum price sum is 2. It can be proved that 2 is the maximum cost.

**Constraints:**

*   <code>1 <= n <= 10<sup>5</sup></code>
*   `edges.length == n - 1`
*   <code>0 <= a<sub>i</sub>, b<sub>i</sub> <= n - 1</code>
*   `edges` represents a valid tree.
*   `price.length == n`
*   <code>1 <= price[i] <= 10<sup>5</sup></code>

## Solution

```kotlin
class Solution {
    private lateinit var tree: Array<ArrayList<Int>?>
    private lateinit var price: IntArray
    private var res: Long = 0
    private lateinit var visited: BooleanArray

    fun maxOutput(n: Int, edges: Array<IntArray>, price: IntArray): Long {
        if (n == 1) {
            return 0
        }
        this.price = price
        tree = arrayOfNulls(n)
        for (i in 0 until n) {
            tree[i] = ArrayList()
        }
        for (e in edges) {
            tree[e[0]]?.add(e[1])
            tree[e[1]]?.add(e[0])
        }
        visited = BooleanArray(n)
        visited[0] = true
        dfs(0)
        return res
    }

    // return long[]{longest path with leaf, longest path without leaf}
    private fun dfs(node: Int): LongArray {
        if (tree[node]?.size == 1 && node != 0) {
            return longArrayOf(price[node].toLong(), 0)
        }
        var i0 = -1
        var i1 = -1
        var l0: Long = 0
        var l1: Long = 0
        var s0: Long = 0
        var s1: Long = 0
        for (child in tree[node]!!) {
            if (visited[child]) {
                continue
            }
            visited[child] = true
            val sub = dfs(child)
            if (sub[0] >= l0) {
                s0 = l0
                l0 = sub[0]
                i0 = child
            } else if (sub[0] > s0) {
                s0 = sub[0]
            }
            if (sub[1] >= l1) {
                s1 = l1
                l1 = sub[1]
                i1 = child
            } else if (sub[1] > s1) {
                s1 = sub[1]
            }
        }
        res = if (s0 == 0L) {
            // only one child. case: example 2
            Math.max(res, Math.max(l0, l1 + price[node]))
        } else {
            val path = if (i0 != i1) price[node] + l0 + l1 else price[node] + Math.max(l0 + s1, s0 + l1)
            Math.max(res, path)
        }
        return longArrayOf(l0 + price[node], l1 + price[node])
    }
}
```