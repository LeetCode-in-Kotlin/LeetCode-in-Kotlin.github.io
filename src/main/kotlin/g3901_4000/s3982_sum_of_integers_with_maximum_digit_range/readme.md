[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3982\. Sum of Integers with Maximum Digit Range

Easy

You are given an integer array `nums`.

The **digit range** of an integer is defined as the difference between its **largest** digit and **smallest** digit.

For example, the digit range of 5724 is `7 - 2 = 5`.

Return the sum of all integers in `nums` whose **digit range** is equal to the **maximum digit range** among all integers in the array.

**Example 1:**

**Input:** nums = [5724,111,350]

**Output:** 6074

**Explanation:**

| `i` | `nums[i]` | Largest | Smallest | Digit Range |
| --- | --- | --- | --- | --- |
| 0 | 5724 | 7 | 2 | 5 |
| 1 | 111 | 1 | 1 | 0 |
| 2 | 350 | 5 | 0 | 5 |

The maximum digit range is 5. The integers with this digit range are 5724 and 350, so the answer is `5724 + 350 = 6074`.

**Example 2:**

**Input:** nums = [90,900]

**Output:** 990

**Explanation:**

| `i` | `nums[i]` | Largest | Smallest | Digit Range |
| --- | --- | --- | --- | --- |
| 0 | 90 | 9 | 0 | 9 |
| 1 | 900 | 9 | 0 | 9 |

The maximum digit range is 9. Both integers have this digit range, so the answer is `90 + 900 = 990`.

**Constraints:**

*   `1 <= nums.length <= 100`
*   <code>10 <= nums[i] <= 10<sup>5</sup></code>

## Solution

```kotlin
import kotlin.math.max
import kotlin.math.min

class Solution {
    fun maxDigitRange(nums: IntArray): Int {
        val n = nums.size
        val range = nums.clone()
        var maxRange = Int.MIN_VALUE
        for (i in 0..<n) {
            var x = range[i]
            var mn = Int.MAX_VALUE
            var mx = Int.MIN_VALUE
            while (x > 0) {
                val d = x % 10
                mn = min(mn, d)
                mx = max(mx, d)
                x /= 10
            }
            range[i] = mx - mn
            maxRange = max(maxRange, range[i])
        }
        var ans = 0
        for (i in 0..<n) {
            if (range[i] == maxRange) {
                ans += nums[i]
            }
        }
        return ans
    }
}
```