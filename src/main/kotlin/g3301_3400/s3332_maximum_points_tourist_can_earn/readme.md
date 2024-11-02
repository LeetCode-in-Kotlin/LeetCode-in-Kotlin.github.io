[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3332\. Maximum Points Tourist Can Earn

Medium

You are given two integers, `n` and `k`, along with two 2D integer arrays, `stayScore` and `travelScore`.

A tourist is visiting a country with `n` cities, where each city is **directly** connected to every other city. The tourist's journey consists of **exactly** `k` **0-indexed** days, and they can choose **any** city as their starting point.

Each day, the tourist has two choices:

*   **Stay in the current city**: If the tourist stays in their current city `curr` during day `i`, they will earn `stayScore[i][curr]` points.
*   **Move to another city**: If the tourist moves from their current city `curr` to city `dest`, they will earn `travelScore[curr][dest]` points.

Return the **maximum** possible points the tourist can earn.

**Example 1:**

**Input:** n = 2, k = 1, stayScore = \[\[2,3]], travelScore = \[\[0,2],[1,0]]

**Output:** 3

**Explanation:**

The tourist earns the maximum number of points by starting in city 1 and staying in that city.

**Example 2:**

**Input:** n = 3, k = 2, stayScore = \[\[3,4,2],[2,1,2]], travelScore = \[\[0,2,1],[2,0,4],[3,2,0]]

**Output:** 8

**Explanation:**

The tourist earns the maximum number of points by starting in city 1, staying in that city on day 0, and traveling to city 2 on day 1.

**Constraints:**

*   `1 <= n <= 200`
*   `1 <= k <= 200`
*   `n == travelScore.length == travelScore[i].length == stayScore[i].length`
*   `k == stayScore.length`
*   `1 <= stayScore[i][j] <= 100`
*   `0 <= travelScore[i][j] <= 100`
*   `travelScore[i][i] == 0`

## Solution

```kotlin
import kotlin.math.max

class Solution {
    fun maxScore(n: Int, k: Int, stayScores: Array<IntArray>, travelScores: Array<IntArray>): Int {
        // dp[day][city]
        val dp = Array<IntArray>(k + 1) { IntArray(n) }
        var result = 0
        for (day in k - 1 downTo 0) {
            for (city in 0 until n) {
                val stayScore = stayScores[day][city] + dp[day + 1][city]
                var travelScore = 0
                for (nextCity in 0 until n) {
                    val nextScore = travelScores[city][nextCity] + dp[day + 1][nextCity]
                    travelScore = max(nextScore, travelScore)
                }
                dp[day][city] = max(stayScore, travelScore)
                result = max(dp[day][city], result)
            }
        }
        return result
    }
}
```