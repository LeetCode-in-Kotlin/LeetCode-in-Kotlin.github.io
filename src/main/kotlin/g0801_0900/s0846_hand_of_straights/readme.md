[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 846\. Hand of Straights

Medium

Alice has some number of cards and she wants to rearrange the cards into groups so that each group is of size `groupSize`, and consists of `groupSize` consecutive cards.

Given an integer array `hand` where `hand[i]` is the value written on the <code>i<sup>th</sup></code> card and an integer `groupSize`, return `true` if she can rearrange the cards, or `false` otherwise.

**Example 1:**

**Input:** hand = [1,2,3,6,2,3,4,7,8], groupSize = 3

**Output:** true

**Explanation:** Alice's hand can be rearranged as [1,2,3],[2,3,4],[6,7,8]

**Example 2:**

**Input:** hand = [1,2,3,4,5], groupSize = 4

**Output:** false

**Explanation:** Alice's hand can not be rearranged into groups of 4.

**Constraints:**

*   <code>1 <= hand.length <= 10<sup>4</sup></code>
*   <code>0 <= hand[i] <= 10<sup>9</sup></code>
*   `1 <= groupSize <= hand.length`

**Note:** This question is the same as 1296: [https://leetcode.com/problems/divide-array-in-sets-of-k-consecutive-numbers/](https://leetcode.com/problems/divide-array-in-sets-of-k-consecutive-numbers/)

## Solution

```kotlin
class Solution {
    fun isNStraightHand(hand: IntArray, groupSize: Int): Boolean {
        val len = hand.size
        if (len % groupSize != 0) {
            return false
        }
        hand.sort()
        val map: MutableMap<Int, Int> = HashMap()
        for (num in hand) {
            var cnt = map.getOrDefault(num, 0)
            map[num] = ++cnt
        }
        for (num in hand) {
            var cnt = map[num]!!
            if (cnt <= 0) {
                continue
            }
            map[num] = --cnt
            var loop = 1
            while (loop < groupSize) {
                var curCnt = map.getOrDefault(num + loop, 0)
                if (curCnt <= 0) {
                    return false
                }
                map[num + loop] = --curCnt
                ++loop
            }
        }
        return true
    }
}
```