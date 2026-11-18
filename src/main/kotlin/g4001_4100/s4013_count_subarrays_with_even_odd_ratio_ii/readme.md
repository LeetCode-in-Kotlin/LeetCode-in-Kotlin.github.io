[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 4013\. Count Subarrays With Even Odd Ratio II

Hard

You are given an integer array `nums` and two integers `a` and `b`.

For a non-empty subarrays, let:

*   `x` be the number of even elements.
*   `y` be the number of odd elements.

The ratio of even to odd elements in a subarray is defined as `x / y`, where ratios are compared by their exact rational values.

A subarray is considered **valid** if:

*   `y > 0`, and
*   `x / y <= a / b`.

Return the number of valid subarrays in `nums`.

**Example 1:**

**Input:** nums = [1,2,1,2], a = 3, b = 2

**Output:** 7

**Explanation:**

The following are the valid subarrays:

| Subarray | Values | Even Count | Odd Count | Ratio |
| --- | --- | --- | --- | --- |
| `nums[0..0]` | `[1]` | 0 | 1 | `0 / 1` |
| `nums[0..1]` | `[1, 2]` | 1 | 1 | `1 / 1` |
| `nums[0..2]` | `[1, 2, 1]` | 1 | 2 | `1 / 2` |
| `nums[0..3]` | `[1, 2, 1, 2]` | 2 | 2 | `2 / 2` |
| `nums[1..2]` | `[2, 1]` | 1 | 1 | `1 / 1` |
| `nums[2..2]` | `[1]` | 0 | 1 | `0 / 1` |
| `nums[2..3]` | `[1, 2]` | 1 | 1 | `1 / 1` |

Thus, the number of valid subarrays is 7.

**Example 2:**

**Input:** nums = [2,2,1], a = 2, b = 1

**Output:** 3

**Explanation:**

The following are the valid subarrays:

| Subarray | Values | Even Count | Odd Count | Ratio |
| --- | --- | --- | --- | --- |
| `nums[0..2]` | `[2, 2, 1]` | 2 | 1 | `2 / 1` |
| `nums[1..2]` | `[2, 1]` | 1 | 1 | `1 / 1` |
| `nums[2..2]` | `[1]` | 0 | 1 | `0 / 1` |

Thus, the number of valid subarrays is 3.

**Example 3:**

**Input:** nums = [2,2,2], a = 1, b = 1

**Output:** 0

**Explanation:**

Every subarray contains 0 odd numbers, so no subarray is valid.

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>9</sup></code>
*   <code>1 <= a, b <= 10<sup>9</sup></code>

## Solution

```kotlin
class Solution {
    fun countRatioSubarrays(nums: IntArray, a: Int, b: Int): Long {
        val n = nums.size
        val arr = LongArray(n + 1)
        val temp = LongArray(n + 1)
        var sum: Long = 0
        for (i in 0..<n) {
            sum += (if (nums[i] % 2 == 1) a else -b).toLong()
            temp[i + 1] = sum
            arr[i + 1] = temp[i + 1]
        }
        return mergeSort(0, n, arr, temp)
    }

    private fun mergeSort(l: Int, r: Int, arr: LongArray, temp: LongArray): Long {
        if (l == r) {
            return 0
        }
        val m = (l + r) / 2
        var result = mergeSort(l, m, temp, arr) + mergeSort(m + 1, r, temp, arr)
        var idx = l
        var i = l
        var j = m + 1
        while (i <= m && j <= r) {
            if (temp[i] <= temp[j]) {
                arr[idx++] = temp[i++]
            } else {
                result += (i - l).toLong()
                arr[idx++] = temp[j++]
            }
        }
        while (i <= m) {
            arr[idx++] = temp[i++]
        }
        while (j <= r) {
            result += (i - l).toLong()
            arr[idx++] = temp[j++]
        }
        return result
    }
}
```