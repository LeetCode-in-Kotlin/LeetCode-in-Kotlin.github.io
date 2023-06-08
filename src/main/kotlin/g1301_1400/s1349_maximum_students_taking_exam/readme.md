[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1349\. Maximum Students Taking Exam

Hard

Given a `m * n` matrix `seats` that represent seats distributions in a classroom. If a seat is broken, it is denoted by `'#'` character otherwise it is denoted by a `'.'` character.

Students can see the answers of those sitting next to the left, right, upper left and upper right, but he cannot see the answers of the student sitting directly in front or behind him. Return the **maximum** number of students that can take the exam together without any cheating being possible..

Students must be placed in seats in good condition.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/01/29/image.png)

**Input:** seats = \[\["#",".","#","#",".","#"], 
                    [".","#","#","#","#","."], 
                    ["#",".","#","#",".","#"]]

**Output:** 4

**Explanation:** Teacher can place 4 students in available seats so they don't cheat on the exam.

**Example 2:**

**Input:** seats = \[\[".","#"], 
                    ["#","#"], 
                    ["#","."], 
                    ["#","#"], 
                    [".","#"]]

**Output:** 3

**Explanation:** Place all students in available seats.

**Example 3:**

**Input:** seats = \[\["#",".","**.**",".","#"], 
                    ["**.**","#","**.**","#","**.**"], 
                    ["**.**",".","#",".","**.**"], 
                    ["**.**","#","**.**","#","**.**"], 
                    ["#",".","**.**",".","#"]]

**Output:** 10

**Explanation:** Place students in available seats in column 1, 3 and 5.

**Constraints:**

*   `seats` contains only characters `'.' and``'#'.`
*   `m == seats.length`
*   `n == seats[i].length`
*   `1 <= m <= 8`
*   `1 <= n <= 8`

## Solution

```kotlin
class Solution {
    fun maxStudents(seats: Array<CharArray>): Int {
        val m = seats.size
        val n = seats[0].size
        val validRows = IntArray(m)
        for (i in 0 until m) {
            for (j in 0 until n) {
                validRows[i] = (validRows[i] shl 1) + if (seats[i][j] == '.') 1 else 0
            }
        }
        val stateSize = 1 shl n
        val dp = Array(m) { IntArray(stateSize) }
        for (i in 0 until m) {
            dp[i].fill(-1)
        }
        var ans = 0
        for (i in 0 until m) {
            for (j in 0 until stateSize) {
                if (j and validRows[i] == j && j and (j shl 1) == 0) {
                    if (i == 0) {
                        dp[i][j] = Integer.bitCount(j)
                    } else {
                        for (k in 0 until stateSize) {
                            if (k shl 1 and j == 0 && j shl 1 and k == 0 && dp[i - 1][k] != -1) {
                                dp[i][j] = Math.max(dp[i][j], dp[i - 1][k] + Integer.bitCount(j))
                            }
                        }
                    }
                    ans = Math.max(ans, dp[i][j])
                }
            }
        }
        return ans
    }
}
```