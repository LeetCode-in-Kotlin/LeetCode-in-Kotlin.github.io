[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1218\. Longest Arithmetic Subsequence of Given Difference

Medium

Given an integer array `arr` and an integer `difference`, return the length of the longest subsequence in `arr` which is an arithmetic sequence such that the difference between adjacent elements in the subsequence equals `difference`.

A **subsequence** is a sequence that can be derived from `arr` by deleting some or no elements without changing the order of the remaining elements.

**Example 1:**

**Input:** arr = [1,2,3,4], difference = 1

**Output:** 4

**Explanation:** The longest arithmetic subsequence is [1,2,3,4].

**Example 2:**

**Input:** arr = [1,3,5,7], difference = 1

**Output:** 1

**Explanation:** The longest arithmetic subsequence is any single element.

**Example 3:**

**Input:** arr = [1,5,7,8,5,3,4,2,1], difference = -2

**Output:** 4

**Explanation:** The longest arithmetic subsequence is [7,5,3,1].

**Constraints:**

*   <code>1 <= arr.length <= 10<sup>5</sup></code>
*   <code>-10<sup>4</sup> <= arr[i], difference <= 10<sup>4</sup></code>

## Solution

```kotlin
class Solution {
    fun longestSubsequence(arr: IntArray, difference: Int): Int {
        var res = 0
        val dp = IntArray(20001)
        for (j in arr) {
            val cur = j + 10000
            val last = j - difference + 10000
            if (last < 0 || last > 20000) {
                dp[cur] = Math.max(dp[cur], 1)
            } else {
                dp[cur] = Math.max(dp[cur], dp[last] + 1)
            }
            res = Math.max(res, dp[cur])
        }
        return res
    }
}
```