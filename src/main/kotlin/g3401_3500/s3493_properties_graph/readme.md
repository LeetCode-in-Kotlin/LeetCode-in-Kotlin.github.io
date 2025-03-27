[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3493\. Properties Graph

Medium

You are given a 2D integer array `properties` having dimensions `n x m` and an integer `k`.

Define a function `intersect(a, b)` that returns the **number of distinct integers** common to both arrays `a` and `b`.

Construct an **undirected** graph where each index `i` corresponds to `properties[i]`. There is an edge between node `i` and node `j` if and only if `intersect(properties[i], properties[j]) >= k`, where `i` and `j` are in the range `[0, n - 1]` and `i != j`.

Return the number of **connected components** in the resulting graph.

**Example 1:**

**Input:** properties = \[\[1,2],[1,1],[3,4],[4,5],[5,6],[7,7]], k = 1

**Output:** 3

**Explanation:**

The graph formed has 3 connected components:

![](https://assets.leetcode.com/uploads/2025/02/27/image.png)

**Example 2:**

**Input:** properties = \[\[1,2,3],[2,3,4],[4,3,5]], k = 2

**Output:** 1

**Explanation:**

The graph formed has 1 connected component:

![](https://assets.leetcode.com/uploads/2025/02/27/screenshot-from-2025-02-27-23-58-34.png)

**Example 3:**

**Input:** properties = \[\[1,1],[1,1]], k = 2

**Output:** 2

**Explanation:**

`intersect(properties[0], properties[1]) = 1`, which is less than `k`. This means there is no edge between `properties[0]` and `properties[1]` in the graph.

**Constraints:**

*   `1 <= n == properties.length <= 100`
*   `1 <= m == properties[i].length <= 100`
*   `1 <= properties[i][j] <= 100`
*   `1 <= k <= m`

## Solution

```kotlin
import java.util.BitSet

class Solution {
    private lateinit var parent: IntArray

    fun numberOfComponents(properties: Array<IntArray>, k: Int): Int {
        val al = convertToList(properties)
        val n = al.size
        val bs: MutableList<BitSet> = ArrayList<BitSet>(n)
        for (integers in al) {
            val bitset = BitSet(101)
            for (num in integers) {
                bitset.set(num)
            }
            bs.add(bitset)
        }
        parent = IntArray(n)
        for (i in 0..<n) {
            parent[i] = i
        }
        for (i in 0..<n) {
            for (j in i + 1..<n) {
                val temp = bs[i].clone() as BitSet
                temp.and(bs[j])
                val common = temp.cardinality()
                if (common >= k) {
                    unionn(i, j)
                }
            }
        }
        val comps: MutableSet<Int> = HashSet<Int>()
        for (i in 0..<n) {
            comps.add(findp(i))
        }
        return comps.size
    }

    private fun findp(x: Int): Int {
        if (parent[x] != x) {
            parent[x] = findp(parent[x])
        }
        return parent[x]
    }

    private fun unionn(a: Int, b: Int) {
        val pa = findp(a)
        val pb = findp(b)
        if (pa != pb) {
            parent[pa] = pb
        }
    }

    private fun convertToList(arr: Array<IntArray>): MutableList<MutableList<Int>> {
        val list: MutableList<MutableList<Int>> = ArrayList<MutableList<Int>>()
        for (row in arr) {
            val temp: MutableList<Int> = ArrayList<Int>()
            for (num in row) {
                temp.add(num)
            }
            list.add(temp)
        }
        return list
    }
}
```