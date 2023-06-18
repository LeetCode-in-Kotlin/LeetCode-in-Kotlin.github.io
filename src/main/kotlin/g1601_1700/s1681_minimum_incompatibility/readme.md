[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1681\. Minimum Incompatibility

Hard

You are given an integer array `nums` and an integer `k`. You are asked to distribute this array into `k` subsets of **equal size** such that there are no two equal elements in the same subset.

A subset's **incompatibility** is the difference between the maximum and minimum elements in that array.

Return _the **minimum possible sum of incompatibilities** of the_ `k` _subsets after distributing the array optimally, or return_ `-1` _if it is not possible._

A subset is a group integers that appear in the array with no particular order.

**Example 1:**

**Input:** nums = [1,2,1,4], k = 2

**Output:** 4

**Explanation:** The optimal distribution of subsets is [1,2] and [1,4].

The incompatibility is (2-1) + (4-1) = 4.

Note that [1,1] and [2,4] would result in a smaller sum, but the first subset contains 2 equal elements.

**Example 2:**

**Input:** nums = [6,3,8,1,3,1,2,2], k = 4

**Output:** 6

**Explanation:** The optimal distribution of subsets is [1,2], [2,3], [6,8], and [1,3].

The incompatibility is (2-1) + (3-2) + (8-6) + (3-1) = 6.

**Example 3:**

**Input:** nums = [5,3,3,6,3,3], k = 3

**Output:** -1

**Explanation:** It is impossible to distribute nums into 3 subsets where no two elements are equal in the same subset.

**Constraints:**

*   `1 <= k <= nums.length <= 16`
*   `nums.length` is divisible by `k`
*   `1 <= nums[i] <= nums.length`

## Solution

```kotlin
class Solution {
    private class Node {
        var visited: BooleanArray = BooleanArray(17)
        var size = 0
        var min = 20
        var max = 0
    }

    private lateinit var nodes: Array<Node?>
    private var size = 0
    private var result = 1000000
    private var currentSum = 0

    fun minimumIncompatibility(nums: IntArray, k: Int): Int {
        size = nums.size / k
        nodes = arrayOfNulls(k)
        for (i in 0 until k) {
            nodes[i] = Node()
        }
        nums.sort()
        currentSum = 0
        solve(nums, 0)
        return if (result == 1000000) -1 else result
    }

    private fun solve(nums: IntArray, idx: Int) {
        if (idx == nums.size) {
            result = currentSum
            return
        }
        var minSize = size
        var prevMin: Int
        var prevMax: Int
        var diff: Int
        for (node in nodes) {
            if (node!!.size == minSize || node.visited[nums[idx]]) {
                continue
            }
            minSize = node.size
            prevMin = node.min
            prevMax = node.max
            diff = prevMax - prevMin
            node.min = Math.min(node.min, nums[idx])
            node.max = Math.max(node.max, nums[idx])
            node.size++
            node.visited[nums[idx]] = true
            currentSum += if (prevMin == 20) {
                node.max - node.min
            } else {
                node.max - node.min - diff
            }
            if (currentSum < result) {
                solve(nums, idx + 1)
            }
            currentSum -= if (prevMin == 20) {
                node.max - node.min
            } else {
                node.max - node.min - diff
            }
            node.visited[nums[idx]] = false
            node.size--
            node.min = prevMin
            node.max = prevMax
        }
    }
}
```