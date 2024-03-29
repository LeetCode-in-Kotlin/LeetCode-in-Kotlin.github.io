[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1722\. Minimize Hamming Distance After Swap Operations

Medium

You are given two integer arrays, `source` and `target`, both of length `n`. You are also given an array `allowedSwaps` where each <code>allowedSwaps[i] = [a<sub>i</sub>, b<sub>i</sub>]</code> indicates that you are allowed to swap the elements at index <code>a<sub>i</sub></code> and index <code>b<sub>i</sub></code> **(0-indexed)** of array `source`. Note that you can swap elements at a specific pair of indices **multiple** times and in **any** order.

The **Hamming distance** of two arrays of the same length, `source` and `target`, is the number of positions where the elements are different. Formally, it is the number of indices `i` for `0 <= i <= n-1` where `source[i] != target[i]` **(0-indexed)**.

Return _the **minimum Hamming distance** of_ `source` _and_ `target` _after performing **any** amount of swap operations on array_ `source`_._

**Example 1:**

**Input:** source = [1,2,3,4], target = [2,1,4,5], allowedSwaps = \[\[0,1],[2,3]]

**Output:** 1

**Explanation:** source can be transformed the following way: 

- Swap indices 0 and 1: source = [2,1,3,4] 

- Swap indices 2 and 3: source = [2,1,4,3] 
  
The Hamming distance of source and target is 1 as they differ in 1 position: index 3.

**Example 2:**

**Input:** source = [1,2,3,4], target = [1,3,2,4], allowedSwaps = []

**Output:** 2

**Explanation:** There are no allowed swaps. The Hamming distance of source and target is 2 as they differ in 2 positions: index 1 and index 2.

**Example 3:**

**Input:** source = [5,1,2,4,3], target = [1,5,4,2,3], allowedSwaps = \[\[0,4],[4,2],[1,3],[1,4]]

**Output:** 0

**Constraints:**

*   `n == source.length == target.length`
*   <code>1 <= n <= 10<sup>5</sup></code>
*   <code>1 <= source[i], target[i] <= 10<sup>5</sup></code>
*   <code>0 <= allowedSwaps.length <= 10<sup>5</sup></code>
*   `allowedSwaps[i].length == 2`
*   <code>0 <= a<sub>i</sub>, b<sub>i</sub> <= n - 1</code>
*   <code>a<sub>i</sub> != b<sub>i</sub></code>

## Solution

```kotlin
class Solution {
    fun minimumHammingDistance(source: IntArray, target: IntArray, allowedSwaps: Array<IntArray>): Int {
        var i: Int
        val n = source.size
        var weight = 0
        val parent = IntArray(n)
        i = 0
        while (i < n) {
            parent[i] = i
            i++
        }
        for (swap in allowedSwaps) {
            union(swap[0], swap[1], parent)
        }
        val components = HashMap<Int, MutableList<Int>>()
        i = 0
        while (i < n) {
            find(i, parent)
            val list = components.getOrDefault(parent[i], ArrayList())
            list.add(i)
            components[parent[i]] = list
            i++
        }
        for ((_, value) in components) {
            weight += getHammingDistance(source, target, value)
        }
        return weight
    }

    private fun getHammingDistance(source: IntArray, target: IntArray, indices: List<Int>): Int {
        val list1 = HashMap<Int, Int>()
        val list2 = HashMap<Int, Int>()
        for (i in indices) {
            list1[target[i]] = 1 + list1.getOrDefault(target[i], 0)
            list2[source[i]] = 1 + list2.getOrDefault(source[i], 0)
        }
        var size = indices.size
        for ((key, value) in list1) {
            size -= Math.min(value, list2.getOrDefault(key, 0))
        }
        return size
    }

    private fun union(x: Int, y: Int, parent: IntArray) {
        if (x != y) {
            val a = find(x, parent)
            val b = find(y, parent)
            if (a != b) {
                parent[a] = b
            }
        }
    }

    private fun find(x: Int, parent: IntArray): Int {
        var y = x
        while (y != parent[y]) {
            y = parent[y]
        }
        parent[x] = y
        return y
    }
}
```