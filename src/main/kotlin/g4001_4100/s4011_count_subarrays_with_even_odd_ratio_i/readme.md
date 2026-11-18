[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 4011\. Count Subarrays With Even Odd Ratio I

Medium

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

*   `1 <= nums.length <= 1000`
*   `1 <= nums[i] <= 1000`
*   `1 <= a, b <= 1000`

## Solution

```kotlin
class Solution {
    private var count = 0

    fun countRatioSubarrays(nums: IntArray, a: Int, b: Int): Int {
        count = 0
        val n = nums.size
        val p = IntArray(n + 1)
        for (i in 0..<n) {
            p[i + 1] = p[i] + (if (nums[i] % 2 == 0) b else -a)
        }
        mergesort(p)
        return count
    }

    private fun mergesort(p: IntArray) {
        val n = p.size
        if (n < 2) {
            return
        }
        val a = IntArray(n / 2)
        val b = IntArray(n - n / 2)
        var idx = 0
        for (i in a.indices) {
            a[i] = p[idx++]
        }
        for (i in b.indices) {
            b[i] = p[idx++]
        }
        mergesort(a)
        mergesort(b)
        merge(a, b, p)
    }

    private fun merge(a: IntArray, b: IntArray, c: IntArray) {
        var i = 0
        var j = 0
        var k = 0
        while (i < a.size && j < b.size) {
            if (a[i] < b[j]) {
                c[k++] = a[i++]
            } else {
                count += (a.size - i)
                c[k++] = b[j++]
            }
        }
        while (i < a.size) {
            c[k++] = a[i++]
        }
        while (j < b.size) {
            c[k++] = b[j++]
        }
    }
}
```