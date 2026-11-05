[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3985\. Palindromic Subarray Sum

Hard

You are given an integer array `nums`.

Return the maximum possible sum of a non-empty subarrays of `nums` that is a palindrome.

**Example 1:**

**Input:** nums = [10,10]

**Output:** 20

**Explanation:**

The whole array `[10,10]` is a palindrome. Therefore, the maximum sum is `10 + 10 = 20`.

**Example 2:**

**Input:** nums = [1,2,3,2,1,5,6]

**Output:** 9

**Explanation:**

The contiguous subarray `[1,2,3,2,1]` is a palindrome. Its sum is `1 + 2 + 3 + 2 + 1 = 9` and it is the maximum sum.

**Example 3:**

**Input:** nums = [7,1,2,1,7,3,4,3,4]

**Output:** 18

**Explanation:**

The contiguous subarray `[7,1,2,1,7]` is a palindrome. Its sum is `7 + 1 + 2 + 1 + 7 = 18` and it is the maximum sum.

**Example 4:**

**Input:** nums = [1,2,3,4,5]

**Output:** 5

**Explanation:**

No subarray with length greater than 1 is a palindrome. The largest element in the array is 5. Therefore, the answer is 5.

**Example 5:**

**Input:** nums = [1000]

**Output:** 1000

**Explanation:**

The subarray with only one element is a palindrome. Therefore, the answer is 1000.

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>9</sup></code>

## Solution

```kotlin
import kotlin.math.max
import kotlin.math.min

class Solution {
    fun getSum(nums: IntArray): Long {
        val prefix = buildPrefixSum(nums)
        val oddMax = calculateOdd(nums, prefix)
        val evenMax = calculateEven(nums, prefix)
        return max(oddMax, evenMax)
    }

    private fun buildPrefixSum(nums: IntArray): LongArray {
        val n = nums.size
        val prefix = LongArray(n + 1)
        for (i in 0..<n) {
            prefix[i + 1] = prefix[i] + nums[i]
        }
        return prefix
    }

    private fun calculateOdd(nums: IntArray, prefix: LongArray): Long {
        val n = nums.size
        val d1 = IntArray(n)
        var l = 0
        var r = -1
        var ans: Long = 0
        for (i in 0..<n) {
            var k = if (i > r) 1 else min(d1[l + r - i], r - i + 1)
            k = expandOdd(nums, i, k)
            d1[i] = k
            val left = i - k + 1
            val right = i + k - 1
            ans = max(ans, getRangeSum(prefix, left, right))
            if (right > r) {
                l = left
                r = right
            }
        }
        return ans
    }

    private fun expandOdd(nums: IntArray, i: Int, k: Int): Int {
        var k = k
        val n = nums.size
        while (i - k >= 0 && i + k < n && nums[i - k] == nums[i + k]) {
            k++
        }
        return k
    }

    private fun calculateEven(nums: IntArray, prefix: LongArray): Long {
        val n = nums.size
        val d2 = IntArray(n)
        var l = 0
        var r = -1
        var ans: Long = 0
        for (i in 0..<n) {
            var k = if (i > r) 0 else min(d2[l + r - i + 1], r - i + 1)
            k = expandEven(nums, i, k)
            d2[i] = k
            if (k > 0) {
                val left = i - k
                val right = i + k - 1
                ans = max(ans, getRangeSum(prefix, left, right))
                if (right > r) {
                    l = left
                    r = right
                }
            }
        }
        return ans
    }

    private fun expandEven(nums: IntArray, i: Int, k: Int): Int {
        var k = k
        while (i - k - 1 >= 0 && i + k < nums.size && nums[i - k - 1] == nums[i + k]) {
            k++
        }
        return k
    }

    private fun getRangeSum(prefix: LongArray, left: Int, right: Int): Long = prefix[right + 1] - prefix[left]
}
```