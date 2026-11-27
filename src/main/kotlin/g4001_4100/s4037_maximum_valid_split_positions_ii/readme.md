[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 4037\. Maximum Valid Split Positions II

Hard

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

*   <code>2 <= nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>9</sup></code>

## Solution

```kotlin
import kotlin.math.max

class Solution {
    fun maxValidSplits(nums: IntArray): Int {
        val n = nums.size
        val preGcd = IntArray(n)
        var g = 0
        for (i in 0..<n) {
            g = gcd(g, nums[i])
            preGcd[i] = g
        }
        val sufGcd = IntArray(n + 1)
        for (i in n - 1 downTo 0) {
            sufGcd[i] = gcd(sufGcd[i + 1], nums[i])
        }
        val allGcd = sufGcd[0]
        var p = 0
        while (preGcd[p] != allGcd) {
            p++
        }
        var q = n - 1
        while (sufGcd[q] != allGcd) {
            q--
        }
        var ans = max(q - p, 0)
        for (i in 0..<n) {
            if (i > 0 && preGcd[i] == preGcd[i - 1]) {
                continue
            }
            val newG = if (i > 0) gcd(preGcd[i - 1], sufGcd[i + 1]) else sufGcd[i + 1]
            if (newG == allGcd) {
                continue
            }
            g = 0
            for (j in 0..<n) {
                if (j == i) {
                    continue
                }
                g = gcd(g, nums[j])
                if (g == newG) {
                    p = j
                    break
                }
            }
            g = 0
            for (j in n - 1 downTo 0) {
                if (j == i) {
                    continue
                }
                g = gcd(g, nums[j])
                if (g == newG) {
                    q = j
                    break
                }
            }
            var res = q - p
            if (p <= i && i < q) {
                res--
            }
            ans = max(ans, res)
            break
        }
        return ans
    }

    private fun gcd(a: Int, b: Int): Int {
        var a = a
        var b = b
        while (a != 0) {
            val tmp = a
            a = b % a
            b = tmp
        }
        return b
    }
}
```