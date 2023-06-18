[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1723\. Find Minimum Time to Finish All Jobs

Hard

You are given an integer array `jobs`, where `jobs[i]` is the amount of time it takes to complete the <code>i<sup>th</sup></code> job.

There are `k` workers that you can assign jobs to. Each job should be assigned to **exactly** one worker. The **working time** of a worker is the sum of the time it takes to complete all jobs assigned to them. Your goal is to devise an optimal assignment such that the **maximum working time** of any worker is **minimized**.

_Return the **minimum** possible **maximum working time** of any assignment._

**Example 1:**

**Input:** jobs = [3,2,3], k = 3

**Output:** 3

**Explanation:** By assigning each person one job, the maximum time is 3.

**Example 2:**

**Input:** jobs = [1,2,4,7,8], k = 2

**Output:** 11

**Explanation:** Assign the jobs the following way: 

Worker 1: 1, 2, 8 (working time = 1 + 2 + 8 = 11) 

Worker 2: 4, 7 (working time = 4 + 7 = 11) 

The maximum working time is 11.

**Constraints:**

*   `1 <= k <= jobs.length <= 12`
*   <code>1 <= jobs[i] <= 10<sup>7</sup></code>

## Solution

```kotlin
class Solution {
    private var min = Int.MAX_VALUE

    fun minimumTimeRequired(jobs: IntArray, k: Int): Int {
        backtraking(jobs, jobs.size - 1, IntArray(k))
        return min
    }

    private fun backtraking(jobs: IntArray, j: Int, sum: IntArray) {
        val max = getMax(sum)
        if (max >= min) {
            return
        }
        if (j < 0) {
            min = max
            return
        }
        for (i in sum.indices) {
            if (i > 0 && sum[i] == sum[i - 1]) {
                continue
            }
            sum[i] += jobs[j]
            backtraking(jobs, j - 1, sum)
            sum[i] -= jobs[j]
        }
    }

    private fun getMax(sum: IntArray): Int {
        var max = Int.MIN_VALUE
        for (j in sum) {
            max = Math.max(max, j)
        }
        return max
    }
}
```