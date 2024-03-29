[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1518\. Water Bottles

Easy

There are `numBottles` water bottles that are initially full of water. You can exchange `numExchange` empty water bottles from the market with one full water bottle.

The operation of drinking a full water bottle turns it into an empty bottle.

Given the two integers `numBottles` and `numExchange`, return _the **maximum** number of water bottles you can drink_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/07/01/sample_1_1875.png)

**Input:** numBottles = 9, numExchange = 3

**Output:** 13

**Explanation:** You can exchange 3 empty bottles to get 1 full water bottle. Number of water bottles you can drink: 9 + 3 + 1 = 13.

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/07/01/sample_2_1875.png)

**Input:** numBottles = 15, numExchange = 4

**Output:** 19

**Explanation:** You can exchange 4 empty bottles to get 1 full water bottle. Number of water bottles you can drink: 15 + 3 + 1 = 19.

**Constraints:**

*   `1 <= numBottles <= 100`
*   `2 <= numExchange <= 100`

## Solution

```kotlin
class Solution {
    fun numWaterBottles(numBottles: Int, numExchange: Int): Int {
        var drunk = numBottles
        var emptyBottles = numBottles
        while (emptyBottles >= numExchange) {
            val exchangedBottles = emptyBottles / numExchange
            drunk += exchangedBottles
            val unUsedEmptyBottles = emptyBottles % numExchange
            emptyBottles = exchangedBottles + unUsedEmptyBottles
        }
        return drunk
    }
}
```