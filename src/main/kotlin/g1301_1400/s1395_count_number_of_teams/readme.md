[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1395\. Count Number of Teams

Medium

There are `n` soldiers standing in a line. Each soldier is assigned a **unique** `rating` value.

You have to form a team of 3 soldiers amongst them under the following rules:

*   Choose 3 soldiers with index (`i`, `j`, `k`) with rating (`rating[i]`, `rating[j]`, `rating[k]`).
*   A team is valid if: (`rating[i] < rating[j] < rating[k]`) or (`rating[i] > rating[j] > rating[k]`) where (`0 <= i < j < k < n`).

Return the number of teams you can form given the conditions. (soldiers can be part of multiple teams).

**Example 1:**

**Input:** rating = [2,5,3,4,1]

**Output:** 3

**Explanation:** We can form three teams given the conditions. (2,3,4), (5,4,1), (5,3,1). 

**Example 2:**

**Input:** rating = [2,1,3]

**Output:** 0

**Explanation:** We can't form any team given the conditions. 

**Example 3:**

**Input:** rating = [1,2,3,4]

**Output:** 4 

**Constraints:**

*   `n == rating.length`
*   `3 <= n <= 1000`
*   <code>1 <= rating[i] <= 10<sup>5</sup></code>
*   All the integers in `rating` are **unique**.

## Solution

```kotlin
@Suppress("NAME_SHADOWING")
class Solution {
    fun numTeams(rating: IntArray): Int {
        val cp = rating.clone()
        cp.sort()
        // count i, j such that i<j and rating[i]<rating[j]
        val count = Array(cp.size) { IntArray(2) }
        val bit = IntArray(cp.size)
        for (i in rating.indices) {
            count[i][0] = count(bs(cp, rating[i] - 1), bit)
            count[i][1] = i - count[i][0]
            add(bs(cp, rating[i]), bit)
        }
        // reverse count from right to left, that i<j and rating[i]>rating[j]
        val reverseCount = Array(cp.size) { IntArray(2) }
        val reverseBit = IntArray(cp.size)
        for (i in cp.indices.reversed()) {
            reverseCount[i][0] = count(bs(cp, rating[i] - 1), reverseBit)
            reverseCount[i][1] = cp.size - 1 - i - reverseCount[i][0]
            add(bs(cp, rating[i]), reverseBit)
        }
        var result = 0
        for (i in rating.indices) {
            result += count[i][0] * reverseCount[i][1] + count[i][1] * reverseCount[i][0]
        }
        return result
    }

    private fun count(idx: Int, bit: IntArray): Int {
        var idx = idx
        var sum = 0
        while (idx >= 0) {
            sum += bit[idx]
            idx = (idx and idx + 1) - 1
        }
        return sum
    }

    private fun add(idx: Int, bit: IntArray) {
        var idx = idx
        if (idx < 0) {
            return
        }
        while (idx < bit.size) {
            bit[idx] += 1
            idx = idx or idx + 1
        }
    }

    private fun bs(arr: IntArray, `val`: Int): Int {
        var l = 0
        var r = arr.size - 1
        while (l < r) {
            val m = l + (r - l) / 2
            if (arr[m] == `val`) {
                return m
            } else if (arr[m] < `val`) {
                l = m + 1
            } else {
                r = m - 1
            }
        }
        return if (arr[l] > `val`) l - 1 else l
    }
}
```