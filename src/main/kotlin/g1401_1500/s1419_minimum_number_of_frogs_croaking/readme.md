[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1419\. Minimum Number of Frogs Croaking

Medium

You are given the string `croakOfFrogs`, which represents a combination of the string `"croak"` from different frogs, that is, multiple frogs can croak at the same time, so multiple `"croak"` are mixed.

_Return the minimum number of_ different _frogs to finish all the croaks in the given string._

A valid `"croak"` means a frog is printing five letters `'c'`, `'r'`, `'o'`, `'a'`, and `'k'` **sequentially**. The frogs have to print all five letters to finish a croak. If the given string is not a combination of a valid `"croak"` return `-1`.

**Example 1:**

**Input:** croakOfFrogs = "croakcroak"

**Output:** 1

**Explanation:** One frog yelling "croak**"** twice.

**Example 2:**

**Input:** croakOfFrogs = "crcoakroak"

**Output:** 2

**Explanation:** The minimum number of frogs is two. The first frog could yell "**cr**c**oak**roak". The second frog could yell later "cr**c**oak**roak**".

**Example 3:**

**Input:** croakOfFrogs = "croakcrook"

**Output:** -1

**Explanation:** The given string is an invalid combination of "croak**"** from different frogs.

**Constraints:**

*   <code>1 <= croakOfFrogs.length <= 10<sup>5</sup></code>
*   `croakOfFrogs` is either `'c'`, `'r'`, `'o'`, `'a'`, or `'k'`.

## Solution

```kotlin
class Solution {
    fun minNumberOfFrogs(s: String): Int {
        var ans = 0
        val f = IntArray(26)
        for (i in 0 until s.length) {
            f[s[i].code - 'a'.code]++
            if (s[i] == 'k' && checkEnough(f)) {
                reduce(f)
            }
            if (!isValid(f)) {
                return -1
            }
            ans = Math.max(ans, getMax(f))
        }
        return if (isEmpty(f)) ans else -1
    }

    private fun checkEnough(f: IntArray): Boolean {
        return f['c'.code - 'a'.code] > 0 && f['r'.code - 'a'.code] > 0 &&
            f['o'.code - 'a'.code] > 0 && f[0] > 0 && f['k'.code - 'a'.code] > 0
    }

    fun reduce(f: IntArray) {
        f['c'.code - 'a'.code]--
        f['r'.code - 'a'.code]--
        f['o'.code - 'a'.code]--
        f[0]--
        f['k'.code - 'a'.code]--
    }

    private fun getMax(f: IntArray): Int {
        var max = 0
        for (v in f) {
            max = Math.max(max, v)
        }
        return max
    }

    private fun isEmpty(f: IntArray): Boolean {
        for (v in f) {
            if (v > 0) {
                return false
            }
        }
        return true
    }

    private fun isValid(f: IntArray): Boolean {
        if (f['r'.code - 'a'.code] > f['c'.code - 'a'.code]) {
            return false
        }
        if (f['o'.code - 'a'.code] > f['r'.code - 'a'.code]) {
            return false
        }
        return if (f[0] > f['o'.code - 'a'.code]) {
            false
        } else {
            f['k'.code - 'a'.code] <= f[0]
        }
    }
}
```