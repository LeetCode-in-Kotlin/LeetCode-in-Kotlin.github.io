[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3309\. Maximum Possible Number by Binary Concatenation

Medium

You are given an array of integers `nums` of size 3.

Return the **maximum** possible number whose _binary representation_ can be formed by **concatenating** the _binary representation_ of **all** elements in `nums` in some order.

**Note** that the binary representation of any number _does not_ contain leading zeros.

**Example 1:**

**Input:** nums = [1,2,3]

**Output:** 30

**Explanation:**

Concatenate the numbers in the order `[3, 1, 2]` to get the result `"11110"`, which is the binary representation of 30.

**Example 2:**

**Input:** nums = [2,8,16]

**Output:** 1296

**Explanation:**

Concatenate the numbers in the order `[2, 8, 16]` to get the result `"10100010000"`, which is the binary representation of 1296.

**Constraints:**

*   `nums.length == 3`
*   `1 <= nums[i] <= 127`

## Solution

```kotlin
class Solution {
    private var result = "0"

    fun maxGoodNumber(nums: IntArray): Int {
        val visited = BooleanArray(nums.size)
        val sb = StringBuilder()
        solve(nums, visited, 0, sb)
        var score = 0
        var `val`: Int
        for (c in result.toCharArray()) {
            `val` = c.code - '0'.code
            score *= 2
            score += `val`
        }
        return score
    }

    private fun solve(nums: IntArray, visited: BooleanArray, pos: Int, sb: StringBuilder) {
        if (pos == nums.size) {
            val `val` = sb.toString()
            if ((result.length == `val`.length && result.compareTo(`val`) < 0) ||
                `val`.length > result.length
            ) {
                result = `val`
            }
            return
        }
        var cur: String?
        for (i in nums.indices) {
            if (visited[i]) {
                continue
            }
            visited[i] = true
            cur = Integer.toBinaryString(nums[i])
            sb.append(cur)
            solve(nums, visited, pos + 1, sb)
            sb.setLength(sb.length - cur.length)
            visited[i] = false
        }
    }
}
```