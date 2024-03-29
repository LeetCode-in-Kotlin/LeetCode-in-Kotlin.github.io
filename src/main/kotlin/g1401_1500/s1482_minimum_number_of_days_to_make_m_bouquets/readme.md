[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1482\. Minimum Number of Days to Make m Bouquets

Medium

You are given an integer array `bloomDay`, an integer `m` and an integer `k`.

You want to make `m` bouquets. To make a bouquet, you need to use `k` **adjacent flowers** from the garden.

The garden consists of `n` flowers, the <code>i<sup>th</sup></code> flower will bloom in the `bloomDay[i]` and then can be used in **exactly one** bouquet.

Return _the minimum number of days you need to wait to be able to make_ `m` _bouquets from the garden_. If it is impossible to make m bouquets return `-1`.

**Example 1:**

**Input:** bloomDay = [1,10,3,10,2], m = 3, k = 1

**Output:** 3

**Explanation:** Let us see what happened in the first three days. x means flower bloomed and \_ means flower did not bloom in the garden.

We need 3 bouquets each should contain 1 flower. 

After day 1: [x, \_, \_, \_, \_] // we can only make one bouquet. 

After day 2: [x, \_, \_, \_, x] // we can only make two bouquets. 

After day 3: [x, \_, x, \_, x] // we can make 3 bouquets. The answer is 3.

**Example 2:**

**Input:** bloomDay = [1,10,3,10,2], m = 3, k = 2

**Output:** -1

**Explanation:** We need 3 bouquets each has 2 flowers, that means we need 6 flowers. We only have 5 flowers so it is impossible to get the needed bouquets and we return -1.

**Example 3:**

**Input:** bloomDay = [7,7,7,7,12,7,7], m = 2, k = 3

**Output:** 12

**Explanation:** We need 2 bouquets each should have 3 flowers. 

Here is the garden after the 7 and 12 days: 

After day 7: [x, x, x, x, \_, x, x]

We can make one bouquet of the first three flowers that bloomed. We cannot make another bouquet from the last three flowers that bloomed because they are not adjacent. 

After day 12: [x, x, x, x, x, x, x] 

It is obvious that we can make two bouquets in different ways.

**Constraints:**

*   `bloomDay.length == n`
*   <code>1 <= n <= 10<sup>5</sup></code>
*   <code>1 <= bloomDay[i] <= 10<sup>9</sup></code>
*   <code>1 <= m <= 10<sup>6</sup></code>
*   `1 <= k <= n`

## Solution

```kotlin
class Solution {
    fun minDays(bloomDay: IntArray, m: Int, k: Int): Int {
        if (bloomDay.size < m.toLong() * k) return -1
        var minDay = Int.MAX_VALUE
        var maxDay = 0
        for (day in bloomDay) {
            if (day > maxDay) {
                maxDay = day
            }
            if (day < minDay) {
                minDay = day
            }
        }
        var left = minDay
        var right = maxDay
        while (left < right) {
            val mid = left + (right - left) / 2
            if (canMake(bloomDay, m, k, mid)) {
                // search in the left
                right = mid
            } else {
                left = mid + 1
            }
        }
        return right
    }

    private fun canMake(bloomDay: IntArray, m: Int, k: Int, day: Int): Boolean {
        var count = 0
        var bouquets = 0
        for (i in bloomDay.indices) {
            if (bloomDay[i] > day) {
                count = 0
            } else {
                count++
                if (count == k) {
                    bouquets++
                    count = 0
                }
            }
        }
        return bouquets >= m
    }
}
```