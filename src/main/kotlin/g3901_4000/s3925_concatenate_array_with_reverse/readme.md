[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3925\. Concatenate Array With Reverse

Easy

You are given an integer array `nums` of length `n`.

Construct a new array `ans` of length `2 * n` such that the first `n` elements are the same as `nums`, and the next `n` elements are the elements of `nums` in reverse order.

Formally, for `0 <= i <= n - 1`:

*   `ans[i] = nums[i]`
*   `ans[i + n] = nums[n - i - 1]`

Return an integer array `ans`.

**Example 1:**

**Input:** nums = [1,2,3]

**Output:** [1,2,3,3,2,1]

**Explanation:**

The first `n` elements of `ans` are the same as `nums`.

For the next `n = 3` elements, each element is taken from `nums` in reverse order:

*   `ans[3] = nums[2] = 3`
*   `ans[4] = nums[1] = 2`
*   `ans[5] = nums[0] = 1`

Thus, `ans = [1, 2, 3, 3, 2, 1]`.

**Example 2:**

**Input:** nums = [1]

**Output:** [1,1]

**Explanation:**

The array remains the same when reversed. Thus, `ans = [1, 1]`.

**Constraints:**

*   `1 <= nums.length <= 100`
*   `1 <= nums[i] <= 100`

## Solution

```kotlin
class Solution {
    fun concatWithReverse(nums: IntArray): IntArray {
        val n = nums.size
        val ans = IntArray(n * 2)
        var count = 0
        for (i in 0..<n) {
            ans[i] = nums[i]
            count++
        }
        for (i in n - 1 downTo 0) {
            ans[count++] = nums[i]
        }
        return ans
    }
}
```