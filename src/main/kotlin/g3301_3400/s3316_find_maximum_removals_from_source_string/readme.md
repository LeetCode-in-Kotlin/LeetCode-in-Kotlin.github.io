[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3316\. Find Maximum Removals From Source String

Medium

You are given a string `source` of size `n`, a string `pattern` that is a subsequence of `source`, and a **sorted** integer array `targetIndices` that contains **distinct** numbers in the range `[0, n - 1]`.

We define an **operation** as removing a character at an index `idx` from `source` such that:

*   `idx` is an element of `targetIndices`.
*   `pattern` remains a subsequence of `source` after removing the character.

Performing an operation **does not** change the indices of the other characters in `source`. For example, if you remove `'c'` from `"acb"`, the character at index 2 would still be `'b'`.

Return the **maximum** number of _operations_ that can be performed.

**Example 1:**

**Input:** source = "abbaa", pattern = "aba", targetIndices = [0,1,2]

**Output:** 1

**Explanation:**

We can't remove `source[0]` but we can do either of these two operations:

*   Remove `source[1]`, so that `source` becomes `"a_baa"`.
*   Remove `source[2]`, so that `source` becomes `"ab_aa"`.

**Example 2:**

**Input:** source = "bcda", pattern = "d", targetIndices = [0,3]

**Output:** 2

**Explanation:**

We can remove `source[0]` and `source[3]` in two operations.

**Example 3:**

**Input:** source = "dda", pattern = "dda", targetIndices = [0,1,2]

**Output:** 0

**Explanation:**

We can't remove any character from `source`.

**Example 4:**

**Input:** source = "yeyeykyded", pattern = "yeyyd", targetIndices = [0,2,3,4]

**Output:** 2

**Explanation:**

We can remove `source[2]` and `source[3]` in two operations.

**Constraints:**

*   <code>1 <= n == source.length <= 3 * 10<sup>3</sup></code>
*   `1 <= pattern.length <= n`
*   `1 <= targetIndices.length <= n`
*   `targetIndices` is sorted in ascending order.
*   The input is generated such that `targetIndices` contains distinct elements in the range `[0, n - 1]`.
*   `source` and `pattern` consist only of lowercase English letters.
*   The input is generated such that `pattern` appears as a subsequence in `source`.

## Solution

```kotlin
import kotlin.math.max

class Solution {
    fun maxRemovals(source: String, pattern: String, targetIndices: IntArray): Int {
        val sChars = source.toCharArray()
        val sn = sChars.size
        val pChars = ("$pattern#").toCharArray()
        val pn = pattern.length
        var tn = targetIndices.size
        val maxPat = IntArray(tn + 1)
        var i = 0
        var di = 0
        var nextTI = targetIndices[0]
        while (i < sn) {
            val c = sChars[i]
            if (i == nextTI) {
                maxPat[di + 1] = maxPat[di]
                var p = maxPat[di + 1]
                for (j in di downTo 1) {
                    val q = maxPat[j - 1]
                    maxPat[j] = if (c != pChars[p]) q else max((p + 1), q)
                    p = q
                }
                if (c == pChars[p]) {
                    maxPat[0] = p + 1
                }
                nextTI = if (++di < tn) targetIndices[di] else -1
            } else {
                for (j in 0..di) {
                    val p = maxPat[j]
                    if (c == pChars[p]) {
                        maxPat[j] = p + 1
                    }
                }
            }
            i++
        }
        while (maxPat[tn] < pn) {
            tn--
        }
        return tn
    }
}
```