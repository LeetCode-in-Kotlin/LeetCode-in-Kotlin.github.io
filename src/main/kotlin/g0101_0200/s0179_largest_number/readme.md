[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 179\. Largest Number

Medium

Given a list of non-negative integers `nums`, arrange them such that they form the largest number and return it.

Since the result may be very large, so you need to return a string instead of an integer.

**Example 1:**

**Input:** nums = [10,2]

**Output:** "210"

**Example 2:**

**Input:** nums = [3,30,34,5,9]

**Output:** "9534330"

**Constraints:**

*   `1 <= nums.length <= 100`
*   <code>0 <= nums[i] <= 10<sup>9</sup></code>

## Solution

```kotlin
import java.util.Arrays

class Solution {
    fun largestNumber(nums: IntArray): String {
        val n = nums.size
        val s = arrayOfNulls<String>(n)
        for (i in 0 until n) {
            s[i] = nums[i].toString()
        }
        Arrays.sort(s) { a: String?, b: String? ->
            (b + a).compareTo(
                a + b
            )
        }
        val sb = StringBuilder()
        for (str in s) {
            sb.append(str)
        }
        val result = sb.toString()
        return if (result.startsWith("0")) "0" else result
    }
}
```