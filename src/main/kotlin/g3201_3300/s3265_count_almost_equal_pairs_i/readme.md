[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3265\. Count Almost Equal Pairs I

Medium

You are given an array `nums` consisting of positive integers.

We call two integers `x` and `y` in this problem **almost equal** if both integers can become equal after performing the following operation **at most once**:

*   Choose **either** `x` or `y` and swap any two digits within the chosen number.

Return the number of indices `i` and `j` in `nums` where `i < j` such that `nums[i]` and `nums[j]` are **almost equal**.

**Note** that it is allowed for an integer to have leading zeros after performing an operation.

**Example 1:**

**Input:** nums = [3,12,30,17,21]

**Output:** 2

**Explanation:**

The almost equal pairs of elements are:

*   3 and 30. By swapping 3 and 0 in 30, you get 3.
*   12 and 21. By swapping 1 and 2 in 12, you get 21.

**Example 2:**

**Input:** nums = [1,1,1,1,1]

**Output:** 10

**Explanation:**

Every two elements in the array are almost equal.

**Example 3:**

**Input:** nums = [123,231]

**Output:** 0

**Explanation:**

We cannot swap any two digits of 123 or 231 to reach the other.

**Constraints:**

*   `2 <= nums.length <= 100`
*   <code>1 <= nums[i] <= 10<sup>6</sup></code>

## Solution

```kotlin
@Suppress("NAME_SHADOWING")
class Solution {
    fun countPairs(nums: IntArray): Int {
        var ans = 0
        for (i in 0 until nums.size - 1) {
            for (j in i + 1 until nums.size) {
                if (nums[i] == nums[j] ||
                    ((nums[j] - nums[i]) % 9 == 0 && check(nums[i], nums[j]))
                ) {
                    ans++
                }
            }
        }
        return ans
    }

    private fun check(a: Int, b: Int): Boolean {
        var a = a
        var b = b
        val ca = IntArray(10)
        val cb = IntArray(10)
        var d = 0
        while (a > 0 || b > 0) {
            if (a % 10 != b % 10) {
                d++
                if (d > 2) {
                    return false
                }
            }
            ca[a % 10]++
            cb[b % 10]++
            a /= 10
            b /= 10
        }
        return d == 2 && areEqual(ca, cb)
    }

    private fun areEqual(a: IntArray, b: IntArray): Boolean {
        for (i in 0..9) {
            if (a[i] != b[i]) {
                return false
            }
        }
        return true
    }
}
```