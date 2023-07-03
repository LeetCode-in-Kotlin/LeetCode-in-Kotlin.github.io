[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2100\. Find Good Days to Rob the Bank

Medium

You and a gang of thieves are planning on robbing a bank. You are given a **0-indexed** integer array `security`, where `security[i]` is the number of guards on duty on the <code>i<sup>th</sup></code> day. The days are numbered starting from `0`. You are also given an integer `time`.

The <code>i<sup>th</sup></code> day is a good day to rob the bank if:

*   There are at least `time` days before and after the <code>i<sup>th</sup></code> day,
*   The number of guards at the bank for the `time` days **before** `i` are **non-increasing**, and
*   The number of guards at the bank for the `time` days **after** `i` are **non-decreasing**.

More formally, this means day `i` is a good day to rob the bank if and only if `security[i - time] >= security[i - time + 1] >= ... >= security[i] <= ... <= security[i + time - 1] <= security[i + time]`.

Return _a list of **all** days **(0-indexed)** that are good days to rob the bank_. _The order that the days are returned in does **not** matter._

**Example 1:**

**Input:** security = [5,3,3,3,5,6,2], time = 2

**Output:** [2,3]

**Explanation:**

On day 2, we have security[0] >= security[1] >= security[2] <= security[3] <= security[4].

On day 3, we have security[1] >= security[2] >= security[3] <= security[4] <= security[5].

No other days satisfy this condition, so days 2 and 3 are the only good days to rob the bank. 

**Example 2:**

**Input:** security = [1,1,1,1,1], time = 0

**Output:** [0,1,2,3,4]

**Explanation:** Since time equals 0, every day is a good day to rob the bank, so return every day. 

**Example 3:**

**Input:** security = [1,2,3,4,5,6], time = 2

**Output:** []

**Explanation:**

No day has 2 days before it that have a non-increasing number of guards.

Thus, no day is a good day to rob the bank, so return an empty list. 

**Constraints:**

*   <code>1 <= security.length <= 10<sup>5</sup></code>
*   <code>0 <= security[i], time <= 10<sup>5</sup></code>

## Solution

```kotlin
class Solution {
    fun goodDaysToRobBank(security: IntArray, time: Int): List<Int> {
        val n = security.size
        // dec: # of non-increasing elements before [i]
        // inc: # of non-decreasing elements after [i]
        val dec = IntArray(n)
        val inc = IntArray(n)
        for (i in 1 until n) {
            if (security[i] <= security[i - 1]) {
                dec[i] = dec[i - 1] + 1
            }
            // no need for else, because array elements are inited as 0
        }
        for (i in n - 2 downTo 0) {
            if (security[i] <= security[i + 1]) {
                inc[i] = inc[i + 1] + 1
            }
            // no need for else, because array elements are inited as 0
        }
        val res: MutableList<Int> = ArrayList()
        for (i in 0 until n) {
            if (dec[i] >= time && inc[i] >= time) {
                res.add(i)
            }
        }
        return res
    }
}
```