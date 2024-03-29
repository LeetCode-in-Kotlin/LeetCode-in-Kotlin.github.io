[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1578\. Minimum Time to Make Rope Colorful

Medium

Alice has `n` balloons arranged on a rope. You are given a **0-indexed** string `colors` where `colors[i]` is the color of the <code>i<sup>th</sup></code> balloon.

Alice wants the rope to be **colorful**. She does not want **two consecutive balloons** to be of the same color, so she asks Bob for help. Bob can remove some balloons from the rope to make it **colorful**. You are given a **0-indexed** integer array `neededTime` where `neededTime[i]` is the time (in seconds) that Bob needs to remove the <code>i<sup>th</sup></code> balloon from the rope.

Return _the **minimum time** Bob needs to make the rope **colorful**_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/12/13/ballon1.jpg)

**Input:** colors = "abaac", neededTime = [1,2,3,4,5]

**Output:** 3

**Explanation:** In the above image, 'a' is blue, 'b' is red, and 'c' is green.

Bob can remove the blue balloon at index 2. This takes 3 seconds.

There are no longer two consecutive balloons of the same color. Total time = 3.

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/12/13/balloon2.jpg)

**Input:** colors = "abc", neededTime = [1,2,3]

**Output:** 0

**Explanation:** The rope is already colorful. Bob does not need to remove any balloons from the rope.

**Example 3:**

![](https://assets.leetcode.com/uploads/2021/12/13/balloon3.jpg)

**Input:** colors = "aabaa", neededTime = [1,2,3,4,1]

**Output:** 2

**Explanation:** Bob will remove the ballons at indices 0 and 4. Each ballon takes 1 second to remove.

There are no longer two consecutive balloons of the same color. Total time = 1 + 1 = 2.

**Constraints:**

*   `n == colors.length == neededTime.length`
*   <code>1 <= n <= 10<sup>5</sup></code>
*   <code>1 <= neededTime[i] <= 10<sup>4</sup></code>
*   `colors` contains only lowercase English letters.

## Solution

```kotlin
class Solution {
    fun minCost(colors: String, neededTime: IntArray): Int {
        val str = colors.toCharArray()
        var minCost = 0
        for (i in 1 until str.size) {
            if (str[i] == str[i - 1]) {
                // accrue the cost of deletion for the lower duplicate
                minCost += Math.min(neededTime[i], neededTime[i - 1])
                // keep the cost of the higher duplicate for next iteration
                neededTime[i] = Math.max(neededTime[i], neededTime[i - 1])
            }
        }
        return minCost
    }
}
```