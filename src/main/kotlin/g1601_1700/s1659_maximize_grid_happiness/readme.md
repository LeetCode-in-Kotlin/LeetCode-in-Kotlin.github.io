[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1659\. Maximize Grid Happiness

Hard

You are given four integers, `m`, `n`, `introvertsCount`, and `extrovertsCount`. You have an `m x n` grid, and there are two types of people: introverts and extroverts. There are `introvertsCount` introverts and `extrovertsCount` extroverts.

You should decide how many people you want to live in the grid and assign each of them one grid cell. Note that you **do not** have to have all the people living in the grid.

The **happiness** of each person is calculated as follows:

*   Introverts **start** with `120` happiness and **lose** `30` happiness for each neighbor (introvert or extrovert).
*   Extroverts **start** with `40` happiness and **gain** `20` happiness for each neighbor (introvert or extrovert).

Neighbors live in the directly adjacent cells north, east, south, and west of a person's cell.

The **grid happiness** is the **sum** of each person's happiness. Return _the **maximum possible grid happiness**._

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/11/05/grid_happiness.png)

**Input:** m = 2, n = 3, introvertsCount = 1, extrovertsCount = 2

**Output:** 240

**Explanation:** Assume the grid is 1-indexed with coordinates (row, column).

We can put the introvert in cell (1,1) and put the extroverts in cells (1,3) and (2,3).

- Introvert at (1,1) happiness: 120 (starting happiness) - (0 \* 30) (0 neighbors) = 120

- Extrovert at (1,3) happiness: 40 (starting happiness) + (1 \* 20) (1 neighbor) = 60

- Extrovert at (2,3) happiness: 40 (starting happiness) + (1 \* 20) (1 neighbor) = 60

The grid happiness is 120 + 60 + 60 = 240.

The above figure shows the grid in this example with each person's happiness. The introvert stays in the light green cell while the extroverts live on the light purple cells.

**Example 2:**

**Input:** m = 3, n = 1, introvertsCount = 2, extrovertsCount = 1

**Output:** 260

**Explanation:** Place the two introverts in (1,1) and (3,1) and the extrovert at (2,1).

- Introvert at (1,1) happiness: 120 (starting happiness) - (1 \* 30) (1 neighbor) = 90

- Extrovert at (2,1) happiness: 40 (starting happiness) + (2 \* 20) (2 neighbors) = 80

- Introvert at (3,1) happiness: 120 (starting happiness) - (1 \* 30) (1 neighbor) = 90

The grid happiness is 90 + 80 + 90 = 260.

**Example 3:**

**Input:** m = 2, n = 2, introvertsCount = 4, extrovertsCount = 0

**Output:** 240

**Constraints:**

*   `1 <= m, n <= 5`
*   `0 <= introvertsCount, extrovertsCount <= min(m * n, 6)`

## Solution

```kotlin
class Solution {
    private var m = 0
    private var n = 0
    private lateinit var dp: Array<Array<Array<Array<IntArray>>>>
    private val notPlace = 0
    private val intro = 1
    private val extro = 2
    private var mod = 0

    fun getMaxGridHappiness(m: Int, n: Int, introvertsCount: Int, extrovertsCount: Int): Int {
        this.m = m
        this.n = n
        val numOfState = Math.pow(3.0, n.toDouble()).toInt()
        dp = Array(m) {
            Array(n) {
                Array(introvertsCount + 1) {
                    Array(extrovertsCount + 1) { IntArray(numOfState) }
                }
            }
        }
        mod = numOfState / 3
        return dfs(0, 0, introvertsCount, extrovertsCount, 0)
    }

    private fun dfs(x: Int, y: Int, ic: Int, ec: Int, state: Int): Int {
        if (x == m) {
            return 0
        } else if (y == n) {
            return dfs(x + 1, 0, ic, ec, state)
        }
        if (dp[x][y][ic][ec][state] != 0) {
            return dp[x][y][ic][ec][state]
        }
        // 1 - not place
        var max = dfs(x, y + 1, ic, ec, state % mod * 3)
        val up = state / mod
        val left = state % 3
        // 2 - place intro
        if (ic > 0) {
            var temp = 120
            if (x > 0 && up != notPlace) {
                temp -= 30
                temp += if (up == intro) -30 else 20
            }
            if (y > 0 && left != notPlace) {
                temp -= 30
                temp += if (left == intro) -30 else 20
            }
            var nextState = state
            nextState %= mod
            nextState *= 3
            nextState += intro
            max = Math.max(max, temp + dfs(x, y + 1, ic - 1, ec, nextState))
        }
        // 3 - place extro
        if (ec > 0) {
            var temp = 40
            if (x > 0 && up != notPlace) {
                temp += 20
                temp += if (up == intro) -30 else 20
            }
            if (y > 0 && left != notPlace) {
                temp += 20
                temp += if (left == intro) -30 else 20
            }
            var nextState = state
            nextState %= mod
            nextState *= 3
            nextState += extro
            max = Math.max(max, temp + dfs(x, y + 1, ic, ec - 1, nextState))
        }
        dp[x][y][ic][ec][state] = max
        return max
    }
}
```