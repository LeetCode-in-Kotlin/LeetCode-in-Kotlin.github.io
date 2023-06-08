[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1335\. Minimum Difficulty of a Job Schedule

Hard

You want to schedule a list of jobs in `d` days. Jobs are dependent (i.e To work on the <code>i<sup>th</sup></code> job, you have to finish all the jobs `j` where `0 <= j < i`).

You have to finish **at least** one task every day. The difficulty of a job schedule is the sum of difficulties of each day of the `d` days. The difficulty of a day is the maximum difficulty of a job done on that day.

You are given an integer array `jobDifficulty` and an integer `d`. The difficulty of the <code>i<sup>th</sup></code> job is `jobDifficulty[i]`.

Return _the minimum difficulty of a job schedule_. If you cannot find a schedule for the jobs return `-1`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/01/16/untitled.png)

**Input:** jobDifficulty = [6,5,4,3,2,1], d = 2

**Output:** 7

**Explanation:** First day you can finish the first 5 jobs, total difficulty = 6. 

Second day you can finish the last job, total difficulty = 1.

The difficulty of the schedule = 6 + 1 = 7

**Example 2:**

**Input:** jobDifficulty = [9,9,9], d = 4

**Output:** -1

**Explanation:** If you finish a job per day you will still have a free day. you cannot find a schedule for the given jobs.

**Example 3:**

**Input:** jobDifficulty = [1,1,1], d = 3

**Output:** 3

**Explanation:** The schedule is one job per day. total difficulty will be 3.

**Constraints:**

*   `1 <= jobDifficulty.length <= 300`
*   `0 <= jobDifficulty[i] <= 1000`
*   `1 <= d <= 10`

## Solution

```kotlin
class Solution {
    fun minDifficulty(jobDifficulty: IntArray, d: Int): Int {
        val totalJobs = jobDifficulty.size
        if (totalJobs < d) {
            return -1
        }
        val maxJobsOneDay = totalJobs - d + 1
        val map = IntArray(totalJobs)
        var maxDiff = Int.MIN_VALUE
        for (k in totalJobs - 1 downTo totalJobs - 1 - maxJobsOneDay + 1) {
            maxDiff = Math.max(maxDiff, jobDifficulty[k])
            map[k] = maxDiff
        }
        for (day in d - 1 downTo 1) {
            val maxEndIndex = totalJobs - 1 - (d - day)
            val maxStartIndex = maxEndIndex - maxJobsOneDay + 1
            for (startIndex in maxStartIndex..maxEndIndex) {
                map[startIndex] = Int.MAX_VALUE
                var maxDiffOfTheDay = Int.MIN_VALUE
                for (endIndex in startIndex..maxEndIndex) {
                    maxDiffOfTheDay = Math.max(maxDiffOfTheDay, jobDifficulty[endIndex])
                    val totalDiff = maxDiffOfTheDay + map[endIndex + 1]
                    map[startIndex] = Math.min(map[startIndex], totalDiff)
                }
            }
        }
        return map[0]
    }
}
```