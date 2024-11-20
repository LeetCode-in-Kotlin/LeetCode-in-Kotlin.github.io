[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1473\. Paint House III

Hard

There is a row of `m` houses in a small city, each house must be painted with one of the `n` colors (labeled from `1` to `n`), some houses that have been painted last summer should not be painted again.

A neighborhood is a maximal group of continuous houses that are painted with the same color.

*   For example: `houses = [1,2,2,3,3,2,1,1]` contains `5` neighborhoods `[{1}, {2,2}, {3,3}, {2}, {1,1}]`.

Given an array `houses`, an `m x n` matrix `cost` and an integer `target` where:

*   `houses[i]`: is the color of the house `i`, and `0` if the house is not painted yet.
*   `cost[i][j]`: is the cost of paint the house `i` with the color `j + 1`.

Return _the minimum cost of painting all the remaining houses in such a way that there are exactly_ `target` _neighborhoods_. If it is not possible, return `-1`.

**Example 1:**

**Input:** houses = [0,0,0,0,0], cost = \[\[1,10],[10,1],[10,1],[1,10],[5,1]], m = 5, n = 2, target = 3

**Output:** 9

**Explanation:** Paint houses of this way [1,2,2,1,1] 

This array contains target = 3 neighborhoods, [{1}, {2,2}, {1,1}]. 

Cost of paint all houses (1 + 1 + 1 + 1 + 5) = 9.

**Example 2:**

**Input:** houses = [0,2,1,2,0], cost = \[\[1,10],[10,1],[10,1],[1,10],[5,1]], m = 5, n = 2, target = 3

**Output:** 11

**Explanation:** Some houses are already painted, Paint the houses of this way [2,2,1,2,2] 

This array contains target = 3 neighborhoods, [{2,2}, {1}, {2,2}].

Cost of paint the first and last house (10 + 1) = 11.

**Example 3:**

**Input:** houses = [3,1,2,3], cost = \[\[1,1,1],[1,1,1],[1,1,1],[1,1,1]], m = 4, n = 3, target = 3

**Output:** -1

**Explanation:** Houses are already painted with a total of 4 neighborhoods [{3},{1},{2},{3}] different of target = 3.

**Constraints:**

*   `m == houses.length == cost.length`
*   `n == cost[i].length`
*   `1 <= m <= 100`
*   `1 <= n <= 20`
*   `1 <= target <= m`
*   `0 <= houses[i] <= n`
*   <code>1 <= cost[i][j] <= 10<sup>4</sup></code>

## Solution

```kotlin
class Solution {
    private lateinit var prev: Array<IntArray>
    private lateinit var curr: Array<IntArray>
    private lateinit var mins: Array<IntArray>

    fun minCost(houses: IntArray, cost: Array<IntArray>, m: Int, n: Int, target: Int): Int {
        prev = Array(n) { IntArray(target) }
        curr = Array(n) { IntArray(target) }
        mins = Array(2) { IntArray(target) }
        for (i in 0 until m) calculate(i, houses, cost, n, target)
        var min = Int.MAX_VALUE
        for (i in 0 until n) min = Math.min(min, curr[i][target - 1])
        return if (min == Int.MAX_VALUE) -1 else min
    }

    private fun calculate(house: Int, houses: IntArray, cost: Array<IntArray>, n: Int, target: Int) {
        swap()
        calculateMins(n, target)
        if (houses[house] > 0) {
            costInPaintedHouse(house, houses, cost, target)
        } else {
            costNotPaintedHouse(
                house,
                cost,
                target,
            )
        }
    }

    private fun costInPaintedHouse(house: Int, houses: IntArray, cost: Array<IntArray>, target: Int) {
        val color = houses[house] - 1
        for (i in cost[house].indices) {
            val group = Math.min(target - 1, house)
            val newG = house == group
            if (i == color) {
                curr[i][0] = prev[i][0]
                for (j in 1..group) {
                    curr[i][j] = if (mins[0][j - 1] == prev[i][j - 1]) mins[1][j - 1] else mins[0][j - 1]
                    curr[i][j] = if (newG && j == group) curr[i][j] else Math.min(curr[i][j], prev[i][j])
                }
            } else {
                for (j in 0..group) curr[i][j] = Int.MAX_VALUE
            }
        }
    }

    private fun costNotPaintedHouse(house: Int, cost: Array<IntArray>, target: Int) {
        for (i in cost[house].indices) {
            val group = Math.min(target - 1, house)
            val newG = house == group
            curr[i][0] = if (prev[i][0] == Int.MAX_VALUE) prev[i][0] else prev[i][0] + cost[house][i]
            for (j in 1..group) {
                curr[i][j] = if (mins[0][j - 1] == prev[i][j - 1]) mins[1][j - 1] else mins[0][j - 1]
                curr[i][j] = if (newG && j == group) curr[i][j] else Math.min(curr[i][j], prev[i][j])
                curr[i][j] = if (curr[i][j] == Int.MAX_VALUE) Int.MAX_VALUE else curr[i][j] + cost[house][i]
            }
        }
    }

    private fun swap() {
        val temp = prev
        prev = curr
        curr = temp
    }

    private fun calculateMins(n: Int, target: Int) {
        for (i in 0 until target - 1) {
            mins[0][i] = prev[0][i]
            mins[1][i] = Int.MAX_VALUE
            for (j in 1 until n) {
                if (prev[j][i] <= mins[0][i]) {
                    mins[1][i] = mins[0][i]
                    mins[0][i] = prev[j][i]
                } else if (prev[j][i] <= mins[1][i]) mins[1][i] = prev[j][i]
            }
        }
    }
}
```