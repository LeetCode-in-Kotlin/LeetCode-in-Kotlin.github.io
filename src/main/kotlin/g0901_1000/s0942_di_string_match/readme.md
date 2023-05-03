[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 942\. DI String Match

Easy

A permutation `perm` of `n + 1` integers of all the integers in the range `[0, n]` can be represented as a string `s` of length `n` where:

*   `s[i] == 'I'` if `perm[i] < perm[i + 1]`, and
*   `s[i] == 'D'` if `perm[i] > perm[i + 1]`.

Given a string `s`, reconstruct the permutation `perm` and return it. If there are multiple valid permutations perm, return **any of them**.

**Example 1:**

**Input:** s = "IDID"

**Output:** [0,4,1,3,2]

**Example 2:**

**Input:** s = "III"

**Output:** [0,1,2,3]

**Example 3:**

**Input:** s = "DDI"

**Output:** [3,2,0,1]

**Constraints:**

*   <code>1 <= s.length <= 10<sup>5</sup></code>
*   `s[i]` is either `'I'` or `'D'`.

## Solution

```kotlin
class Solution {
    fun diStringMatch(s: String): IntArray {
        val arr = IntArray(s.length + 1)
        var max = s.length
        for (i in s.indices) {
            if (s[i] == 'D') {
                arr[i] = max
                max--
            }
        }
        run {
            var i = s.length - 1
            while (i >= 0 && max > 0) {
                if (s[i] == 'I' && arr[i + 1] == 0) {
                    arr[i + 1] = max
                    max--
                }
                i--
            }
        }
        var i = 0
        while (i < arr.size && max > 0) {
            if (arr[i] == 0) {
                arr[i] = max
                max--
            }
            i++
        }
        return arr
    }
}
```