[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 86\. Partition List

Medium

Given the `head` of a linked list and a value `x`, partition it such that all nodes **less than** `x` come before nodes **greater than or equal** to `x`.

You should **preserve** the original relative order of the nodes in each of the two partitions.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/01/04/partition.jpg)

**Input:** head = [1,4,3,2,5,2], x = 3

**Output:** [1,2,2,4,3,5]

**Example 2:**

**Input:** head = [2,1], x = 2

**Output:** [1,2]

**Constraints:**

*   The number of nodes in the list is in the range `[0, 200]`.
*   `-100 <= Node.val <= 100`
*   `-200 <= x <= 200`

## Solution

```kotlin
class Solution {
    fun isScramble(s1: String, s2: String): Boolean {
        val n = s1.length
        val dp = Array(n) { Array(n) { BooleanArray(n + 1) } }
        for (len in 1..n) {
            for (i in 0..n - len) {
                for (j in 0..n - len) {
                    if (len == 1) {
                        dp[i][j][len] = s1[i] == s2[j]
                    } else {
                        var k = 1
                        while (k < len && !dp[i][j][len]) {
                            dp[i][j][len] = (
                                dp[i][j][k] && dp[i + k][j + k][len - k] ||
                                    dp[i][j + len - k][k] && dp[i + k][j][len - k]
                                )
                            k++
                        }
                    }
                }
            }
        }
        return dp[0][0][n]
    }
}
```