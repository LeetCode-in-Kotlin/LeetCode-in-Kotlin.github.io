[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3486\. Longest Special Path II

Hard

You are given an undirected tree rooted at node `0`, with `n` nodes numbered from `0` to `n - 1`. This is represented by a 2D array `edges` of length `n - 1`, where <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>, length<sub>i</sub>]</code> indicates an edge between nodes <code>u<sub>i</sub></code> and <code>v<sub>i</sub></code> with length <code>length<sub>i</sub></code>. You are also given an integer array `nums`, where `nums[i]` represents the value at node `i`.

A **special path** is defined as a **downward** path from an ancestor node to a descendant node in which all node values are **distinct**, except for **at most** one value that may appear twice.

Return an array `result` of size 2, where `result[0]` is the **length** of the **longest** special path, and `result[1]` is the **minimum** number of nodes in all _possible_ **longest** special paths.

**Example 1:**

**Input:** edges = \[\[0,1,1],[1,2,3],[1,3,1],[2,4,6],[4,7,2],[3,5,2],[3,6,5],[6,8,3]], nums = [1,1,0,3,1,2,1,1,0]

**Output:** [9,3]

**Explanation:**

In the image below, nodes are colored by their corresponding values in `nums`.

![](https://assets.leetcode.com/uploads/2025/02/18/e1.png)

The longest special paths are `1 -> 2 -> 4` and `1 -> 3 -> 6 -> 8`, both having a length of 9. The minimum number of nodes across all longest special paths is 3.

**Example 2:**

**Input:** edges = \[\[1,0,3],[0,2,4],[0,3,5]], nums = [1,1,0,2]

**Output:** [5,2]

**Explanation:**

![](https://assets.leetcode.com/uploads/2025/02/18/e2.png)

The longest path is `0 -> 3` consisting of 2 nodes with a length of 5.

**Constraints:**

*   <code>2 <= n <= 5 * 10<sup>4</sup></code>
*   `edges.length == n - 1`
*   `edges[i].length == 3`
*   <code>0 <= u<sub>i</sub>, v<sub>i</sub> < n</code>
*   <code>1 <= length<sub>i</sub> <= 10<sup>3</sup></code>
*   `nums.length == n`
*   <code>0 <= nums[i] <= 5 * 10<sup>4</sup></code>
*   The input is generated such that `edges` represents a valid tree.

## Solution

```kotlin
@Suppress("kotlin:S107")
class Solution {
    fun longestSpecialPath(edges: Array<IntArray>, nums: IntArray): IntArray {
        val ans = intArrayOf(0, 1)
        val graph: MutableMap<Int, MutableList<IntArray>> = HashMap<Int, MutableList<IntArray>>()
        for (edge in edges) {
            val a = edge[0]
            val b = edge[1]
            val c = edge[2]
            graph.computeIfAbsent(a) { _: Int -> ArrayList<IntArray>() }
                .add(intArrayOf(b, c))
            graph.computeIfAbsent(b) { _: Int -> ArrayList<IntArray>() }
                .add(intArrayOf(a, c))
        }
        val costs: MutableList<Int> = ArrayList<Int>()
        val last: MutableMap<Int, Int> = HashMap<Int, Int>()
        dfs(0, 0, -1, ArrayList<Int>(mutableListOf<Int>(0, 0)), nums, graph, costs, last, ans)
        return ans
    }

    private fun dfs(
        node: Int,
        currCost: Int,
        prev: Int,
        left: MutableList<Int>,
        nums: IntArray,
        graph: MutableMap<Int, MutableList<IntArray>>,
        costs: MutableList<Int>,
        last: MutableMap<Int, Int>,
        ans: IntArray,
    ) {
        val nodeColorIndexPrev: Int = last.getOrDefault(nums[node], -1)
        last.put(nums[node], costs.size)
        costs.add(currCost)
        val diff = currCost - costs[left[0]]
        val length = costs.size - left[0]
        if (diff > ans[0] || (diff == ans[0] && length < ans[1])) {
            ans[0] = diff
            ans[1] = length
        }
        for (next in graph.getOrDefault(node, ArrayList<IntArray>())) {
            val nextNode = next[0]
            val nextCost = next[1]
            if (nextNode == prev) {
                continue
            }
            val nextLeft: MutableList<Int> = ArrayList<Int>(left)
            if (last.containsKey(nums[nextNode])) {
                nextLeft.add(last[nums[nextNode]]!! + 1)
            }
            nextLeft.sortWith(Comparator.naturalOrder<Int?>())
            while (nextLeft.size > 2) {
                nextLeft.removeAt(0)
            }
            dfs(nextNode, currCost + nextCost, node, nextLeft, nums, graph, costs, last, ans)
        }
        last.put(nums[node], nodeColorIndexPrev)
        costs.removeAt(costs.size - 1)
    }
}
```