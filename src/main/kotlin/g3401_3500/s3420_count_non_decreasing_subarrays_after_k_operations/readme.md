[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3420\. Count Non-Decreasing Subarrays After K Operations

Hard

You are given an array `nums` of `n` integers and an integer `k`.

For each subarray of `nums`, you can apply **up to** `k` operations on it. In each operation, you increment any element of the subarray by 1.

**Note** that each subarray is considered independently, meaning changes made to one subarray do not persist to another.

Return the number of subarrays that you can make **non-decreasing** after performing at most `k` operations.

An array is said to be **non-decreasing** if each element is greater than or equal to its previous element, if it exists.

**Example 1:**

**Input:** nums = [6,3,1,2,4,4], k = 7

**Output:** 17

**Explanation:**

Out of all 21 possible subarrays of `nums`, only the subarrays `[6, 3, 1]`, `[6, 3, 1, 2]`, `[6, 3, 1, 2, 4]` and `[6, 3, 1, 2, 4, 4]` cannot be made non-decreasing after applying up to k = 7 operations. Thus, the number of non-decreasing subarrays is `21 - 4 = 17`.

**Example 2:**

**Input:** nums = [6,3,1,3,6], k = 4

**Output:** 12

**Explanation:**

The subarray `[3, 1, 3, 6]` along with all subarrays of `nums` with three or fewer elements, except `[6, 3, 1]`, can be made non-decreasing after `k` operations. There are 5 subarrays of a single element, 4 subarrays of two elements, and 2 subarrays of three elements except `[6, 3, 1]`, so there are `1 + 5 + 4 + 2 = 12` subarrays that can be made non-decreasing.

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>9</sup></code>
*   <code>1 <= k <= 10<sup>9</sup></code>

## Solution

```kotlin
class Solution {
    fun countNonDecreasingSubarrays(nums: IntArray, k: Int): Long {
        val n = nums.size
        reverse(nums)
        var res: Long = 0
        var t = k.toLong()
        val q = IntArray(n + 1)
        var hh = 0
        var tt = -1
        var j = 0
        var i = 0
        while (j < n) {
            while (hh <= tt && nums[q[tt]] < nums[j]) {
                val r = q[tt--]
                val l = if (hh <= tt) q[tt] else i - 1
                t -= (r - l).toLong() * (nums[j] - nums[r])
            }
            q[++tt] = j
            while (t < 0) {
                t += (nums[q[hh]] - nums[i]).toLong()
                if (q[hh] == i) hh++
                i++
            }
            res += (j - i + 1).toLong()
            j++
        }
        return res
    }

    private fun reverse(nums: IntArray) {
        val n = nums.size
        var i = 0
        var j = n - 1
        while (i < j) {
            val t = nums[i]
            nums[i] = nums[j]
            nums[j] = t
            i++
            j--
        }
    }
}
```