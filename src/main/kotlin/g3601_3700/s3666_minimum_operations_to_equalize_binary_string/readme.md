[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3666\. Minimum Operations to Equalize Binary String

Hard

You are given a binary string `s`, and an integer `k`.

In one operation, you must choose **exactly** `k` **different** indices and **flip** each `'0'` to `'1'` and each `'1'` to `'0'`.

Return the **minimum** number of operations required to make all characters in the string equal to `'1'`. If it is not possible, return -1.

**Example 1:**

**Input:** s = "110", k = 1

**Output:** 1

**Explanation:**

*   There is one `'0'` in `s`.
*   Since `k = 1`, we can flip it directly in one operation.

**Example 2:**

**Input:** s = "0101", k = 3

**Output:** 2

**Explanation:**

One optimal set of operations choosing `k = 3` indices in each operation is:

*   **Operation 1**: Flip indices `[0, 1, 3]`. `s` changes from `"0101"` to `"1000"`.
*   **Operation 2**: Flip indices `[1, 2, 3]`. `s` changes from `"1000"` to `"1111"`.

Thus, the minimum number of operations is 2.

**Example 3:**

**Input:** s = "101", k = 2

**Output:** \-1

**Explanation:**

Since `k = 2` and `s` has only one `'0'`, it is impossible to flip exactly `k` indices to make all `'1'`. Hence, the answer is -1.

**Constraints:**

*   <code>1 <= s.length <= 10<sup>5</sup></code>
*   `s[i]` is either `'0'` or `'1'`.
*   `1 <= k <= s.length`

## Solution

```kotlin
import kotlin.math.max

class Solution {
    fun minOperations(s: String, k: Int): Int {
        val n = s.length
        var cnt0 = 0
        for (c in s.toCharArray()) {
            if (c == '0') {
                cnt0++
            }
        }
        if (cnt0 == 0) {
            return 0
        }
        if (k == n) {
            return if (cnt0 == n) 1 else -1
        }
        val kP = k and 1
        val needP = cnt0 and 1
        var best = Long.Companion.MAX_VALUE
        for (p in 0..1) {
            if ((p * kP) % 2 != needP) {
                continue
            }
            val mismatch = (if (p == 0) cnt0 else (n - cnt0)).toLong()
            val b1 = (cnt0 + k - 1L) / k
            val b2: Long
            b2 = (mismatch + (n - k) - 1L) / (n - k)
            var lb = max(b1, b2)
            if (lb < 1) {
                lb = 1
            }
            if ((lb and 1L) != p.toLong()) {
                lb++
            }
            if (lb < best) {
                best = lb
            }
        }
        return if (best == Long.Companion.MAX_VALUE) -1 else best.toInt()
    }
}
```