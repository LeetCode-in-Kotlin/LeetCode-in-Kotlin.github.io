[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1203\. Sort Items by Groups Respecting Dependencies

Hard

There are `n` items each belonging to zero or one of `m` groups where `group[i]` is the group that the `i`\-th item belongs to and it's equal to `-1` if the `i`\-th item belongs to no group. The items and the groups are zero indexed. A group can have no item belonging to it.

Return a sorted list of the items such that:

*   The items that belong to the same group are next to each other in the sorted list.
*   There are some relations between these items where `beforeItems[i]` is a list containing all the items that should come before the `i`\-th item in the sorted array (to the left of the `i`\-th item).

Return any solution if there is more than one solution and return an **empty list** if there is no solution.

**Example 1:**

**![](https://assets.leetcode.com/uploads/2019/09/11/1359_ex1.png)**

**Input:** n = 8, m = 2, group = [-1,-1,1,0,0,1,0,-1], beforeItems = \[\[],[6],[5],[6],[3,6],[],[],[]]

**Output:** [6,3,4,1,5,2,0,7]

**Example 2:**

**Input:** n = 8, m = 2, group = [-1,-1,1,0,0,1,0,-1], beforeItems = \[\[],[6],[5],[6],[3],[],[4],[]]

**Output:** []

**Explanation:** This is the same as example 1 except that 4 needs to be before 6 in the sorted list.

**Constraints:**

*   <code>1 <= m <= n <= 3 * 10<sup>4</sup></code>
*   `group.length == beforeItems.length == n`
*   `-1 <= group[i] <= m - 1`
*   `0 <= beforeItems[i].length <= n - 1`
*   `0 <= beforeItems[i][j] <= n - 1`
*   `i != beforeItems[i][j]`
*   `beforeItems[i] `does not contain duplicates elements.

## Solution

```kotlin
import java.util.LinkedList
import java.util.Queue

class Solution {
    fun sortItems(n: Int, m: Int, group: IntArray, beforeItems: List<List<Int>>): IntArray {
        var totalGroups = m
        val indexGroupMap: MutableMap<Int, MutableList<Int>> = HashMap()
        for (i in 0 until n) {
            if (group[i] == -1) {
                group[i] = totalGroups
                indexGroupMap[totalGroups] = ArrayList()
                indexGroupMap[totalGroups]!!.add(i)
                totalGroups++
            } else {
                indexGroupMap.putIfAbsent(group[i], ArrayList())
                indexGroupMap[group[i]]!!.add(i)
            }
        }
        val externalInMap = IntArray(totalGroups)
        val internalInMap = IntArray(n)
        val externalGraph: MutableMap<Int, MutableList<Int>> = HashMap()
        val internalGraph: MutableMap<Int, MutableList<Int>> = HashMap()
        for (i in beforeItems.indices) {
            if (beforeItems[i].isNotEmpty()) {
                val groupNumber = group[i]
                for (j in beforeItems[i].indices) {
                    val prevItem = beforeItems[i][j]
                    val prevGroupNumber = group[prevItem]
                    if (groupNumber == prevGroupNumber) {
                        internalGraph.putIfAbsent(prevItem, ArrayList())
                        internalGraph[prevItem]!!.add(i)
                        internalInMap[i]++
                    } else {
                        externalGraph.putIfAbsent(prevGroupNumber, ArrayList())
                        externalGraph[prevGroupNumber]!!.add(groupNumber)
                        externalInMap[groupNumber]++
                    }
                }
            }
        }
        val externalQueue: Queue<Int> = LinkedList()
        for (i in 0 until totalGroups) {
            if (externalInMap[i] == 0) {
                externalQueue.offer(i)
            }
        }
        val res = IntArray(n)
        var resIndex = 0
        while (externalQueue.isNotEmpty()) {
            val curGroup = externalQueue.poll()
            val internalQueue: Queue<Int> = LinkedList()
            if (indexGroupMap.containsKey(curGroup)) {
                for (item in indexGroupMap[curGroup]!!) {
                    if (internalInMap[item] == 0) {
                        internalQueue.offer(item)
                    }
                }
            }
            while (internalQueue.isNotEmpty()) {
                val curItem = internalQueue.poll()
                res[resIndex] = curItem
                resIndex++
                if (internalGraph.containsKey(curItem)) {
                    for (nextItemInGroup in internalGraph[curItem]!!) {
                        internalInMap[nextItemInGroup]--
                        if (internalInMap[nextItemInGroup] == 0) {
                            internalQueue.offer(nextItemInGroup)
                        }
                    }
                }
            }
            if (externalGraph.containsKey(curGroup)) {
                for (nextGroup in externalGraph[curGroup]!!) {
                    externalInMap[nextGroup]--
                    if (externalInMap[nextGroup] == 0) {
                        externalQueue.offer(nextGroup)
                    }
                }
            }
        }
        return if (resIndex == n) res else intArrayOf()
    }
}
```