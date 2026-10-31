[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3738\. Longest Non-Decreasing Subarray After Replacing at Most One Element

Medium

You are given an integer array `nums`.

You are allowed to replace **at most** one element in the array with any other integer value of your choice.

Return the length of the **longest non-decreasing subarray** that can be obtained after performing at most one replacement.

An array is said to be **non-decreasing** if each element is greater than or equal to its previous one (if it exists).

**Example 1:**

**Input:** nums = [1,2,3,1,2]

**Output:** 4

**Explanation:**

Replacing `nums[3] = 1` with 3 gives the array [1, 2, 3, 3, 2].

The longest non-decreasing subarray is [1, 2, 3, 3], which has a length of 4.

**Example 2:**

**Input:** nums = [2,2,2,2,2]

**Output:** 5

**Explanation:**

All elements in `nums` are equal, so it is already non-decreasing and the entire `nums` forms a subarray of length 5.

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>-10<sup>9</sup> <= nums[i] <= 10<sup>9</sup></code>

## Solution

```kotlin
import kotlin.math.max
import kotlin.math.min

class Solution {
    fun longestSubarray(nums: IntArray): Int {
        val n = nums.size
        if (n < 3) {
            return n
        }
        var i = 0
        var len1: Int
        var len2 = 0
        var ans = 2
        while (i < n) {
            val l = if (i == 0) -1000 * 1000 * 10 else nums[i - 1]
            val r = if (i == n - 1) 1000 * 1000 * 10 else nums[i + 1]
            if (l <= nums[i]) {
                len2++
                ans = max(len2 + 1, ans)
            } else if (r >= nums[i]) {
                var j = i
                len1 = len2
                while (j < n - 1 && nums[j] <= nums[j + 1]) {
                    j++
                }
                len2 = j - i + 1
                ans = max(len2 + 1, ans)
                if (l <= r || (len1 > 1 && nums[i - 2] <= nums[i])) {
                    ans = max(len1 + len2, ans)
                }
                i = j
            } else {
                len2 = 0
            }
            i++
        }
        ans = min(ans, n)
        return ans
    }
}
```