[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 367\. Valid Perfect Square

Easy

Given a **positive** integer _num_, write a function which returns True if _num_ is a perfect square else False.

**Follow up:** **Do not** use any built-in library function such as `sqrt`.

**Example 1:**

**Input:** num = 16

**Output:** true

**Example 2:**

**Input:** num = 14

**Output:** false

**Constraints:**

*   `1 <= num <= 2^31 - 1`

## Solution

```kotlin
class Solution {
    fun isPerfectSquare(num: Int): Boolean {
        if (num == 0) {
            // If num is 0 return false
            return false
        }
        // long datatype can holds huge number.
        var start: Long = 0
        var end = num.toLong()
        var mid: Long
        while (start <= end) {
            // until start is lesser or equal to end do this
            // Finding middle value
            mid = start + (end - start) / 2
            if (mid * mid == num.toLong()) {
                // if mid*mid == num return true
                return true
            } else if (mid * mid < num) {
                // if num is greater than mid*mid then make start = mid + 1
                start = mid + 1
            } else if (mid * mid > num) {
                // if num is lesser than mid*mid then make end = mid - 1
                end = mid - 1
            }
        }
        return false
    }
}
```