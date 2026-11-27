[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 4035\. Maximum Valid Split Positions I

Medium

You are given an integer array `nums`.

You may remove **at most one** element from `nums`. Let `arr` be the array of remaining elements in their original order, and let `m` be its length.

A **split position** `i` of `arr` is **valid** if:

*   `0 <= i < m - 1`, and
*   `gcd(arr[0..i]) == gcd(arr[i + 1..m - 1])`.

An array of length 1 has no valid split positions.

The **score** of `arr` is the number of valid split positions in it.

Return the **maximum possible score** of `arr`.

Here, `gcd(a)` denotes the **greatest common divisor** of all elements in the array `a`.

**Example 1:**

**Input:** nums = [10,30,15,10]

**Output:** 2

**Explanation:**

One optimal solution is to remove `nums[2] = 15`. Then `arr = [10, 30, 10]`.

The split positions are:

| Split Position `i` | `gcd(arr[0..i])` | `gcd(arr[i + 1..m - 1])` |
| --- | --- | --- |
| 0 | 10 | 10 |
| 1 | 10 | 10 |

All split positions are valid. Thus, the answer is 2.

**Example 2:**

**Input:** nums = [2,10,14]

**Output:** 1

**Explanation:**

One optimal solution is to not remove any element. Then `arr = [2, 10, 14]`.

The split positions are:

| Split Position `i` | `gcd(arr[0..i])` | `gcd(arr[i + 1..m - 1])` |
| --- | --- | --- |
| 0 | 2 | 2 |
| 1 | 2 | 14 |

Only the split position at index 0 is valid. Thus, the answer is 1.

**Example 3:**

**Input:** nums = [2,4]

**Output:** 0

**Explanation:**

The only remaining array that has a split position is `arr = [2, 4]`.

The split positions are:

| Split Position `i` | `gcd(arr[0..i])` | `gcd(arr[i + 1..m - 1])` |
| --- | --- | --- |
| 0 | 2 | 4 |

There are no valid split positions. Thus, the answer is 0.

**Constraints:**

*   `2 <= nums.length <= 1000`
*   <code>1 <= nums[i] <= 10<sup>9</sup></code>

## Solution

```kotlin
import kotlin.math.max

class Solution {
    fun maxValidSplits(nums: IntArray): Int {
        val n = nums.size
        val pref = IntArray(n + 1)
        val suf = IntArray(n + 1)
        for (k in 0..<n) {
            pref[k + 1] = gcd(pref[k], nums[k])
        }
        for (k in n - 1 downTo 0) {
            suf[k] = gcd(suf[k + 1], nums[k])
        }
        val total = pref[n]
        var best = 0
        for (k in 0..n) {
            if (pref[k] == suf[k]) {
                best++
            }
        }
        for (i in 0..<n) {
            val g = gcd(pref[i], suf[i + 1])
            if (g == total) {
                continue
            }
            val l = if (pref[i] == g) walk(nums, 0, 0, 1, g) else walk(nums, i + 1, pref[i], 1, g)
            val r =
                if (suf[i + 1] == g) {
                    walk(nums, n - 1, 0, -1, g)
                } else {
                    walk(nums, i - 1, suf[i + 1], -1, g)
                }
            best = max(best, r - l + 2 - (if (l <= i && i <= r) 1 else 0))
        }
        return best
    }

    private fun walk(nums: IntArray, k: Int, cur: Int, step: Int, g: Int): Int {
        var k = k
        var cur = cur
        while (cur != g) {
            cur = gcd(cur, nums[k])
            k += step
        }
        return k
    }

    private fun gcd(a: Int, b: Int): Int {
        var a = a
        var b = b
        while (b != 0) {
            val t = a % b
            a = b
            b = t
        }
        return a
    }
}
```