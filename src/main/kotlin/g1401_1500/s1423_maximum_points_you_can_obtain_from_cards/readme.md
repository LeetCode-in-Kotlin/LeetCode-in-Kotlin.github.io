[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1423\. Maximum Points You Can Obtain from Cards

Medium

There are several cards **arranged in a row**, and each card has an associated number of points. The points are given in the integer array `cardPoints`.

In one step, you can take one card from the beginning or from the end of the row. You have to take exactly `k` cards.

Your score is the sum of the points of the cards you have taken.

Given the integer array `cardPoints` and the integer `k`, return the _maximum score_ you can obtain.

**Example 1:**

**Input:** cardPoints = [1,2,3,4,5,6,1], k = 3

**Output:** 12

**Explanation:** After the first step, your score will always be 1. However, choosing the rightmost card first will maximize your total score. The optimal strategy is to take the three cards on the right, giving a final score of 1 + 6 + 5 = 12.

**Example 2:**

**Input:** cardPoints = [2,2,2], k = 2

**Output:** 4

**Explanation:** Regardless of which two cards you take, your score will always be 4.

**Example 3:**

**Input:** cardPoints = [9,7,7,9,7,7,9], k = 7

**Output:** 55

**Explanation:** You have to take all the cards. Your score is the sum of points of all cards.

**Constraints:**

*   <code>1 <= cardPoints.length <= 10<sup>5</sup></code>
*   <code>1 <= cardPoints[i] <= 10<sup>4</sup></code>
*   `1 <= k <= cardPoints.length`

## Solution

```kotlin
class Solution {
    fun maxScore(cardPoints: IntArray, k: Int): Int {
        var currSum = 0
        var maxSum: Int
        for (i in 0 until k) {
            currSum += cardPoints[i]
        }
        if (k == cardPoints.size) {
            return currSum
        }
        maxSum = currSum
        var r = cardPoints.size - 1
        while (r >= cardPoints.size - k) {
            currSum += cardPoints[r] - cardPoints[k + r - cardPoints.size]
            maxSum = Math.max(currSum, maxSum)
            r--
        }
        return maxSum
    }
}
```