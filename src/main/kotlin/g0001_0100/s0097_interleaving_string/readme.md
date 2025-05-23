[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 97\. Interleaving String

Medium

Given strings `s1`, `s2`, and `s3`, find whether `s3` is formed by an **interleaving** of `s1` and `s2`.

An **interleaving** of two strings `s` and `t` is a configuration where `s` and `t` are divided into `n` and `m` **non-empty** substrings respectively, such that:

*   <code>s = s<sub>1</sub> + s<sub>2</sub> + ... + s<sub>n</sub></code>
*   <code>t = t<sub>1</sub> + t<sub>2</sub> + ... + t<sub>m</sub></code>
*   `|n - m| <= 1`
*   The **interleaving** is <code>s<sub>1</sub> + t<sub>1</sub> + s<sub>2</sub> + t<sub>2</sub> + s<sub>3</sub> + t<sub>3</sub> + ...</code> or <code>t<sub>1</sub> + s<sub>1</sub> + t<sub>2</sub> + s<sub>2</sub> + t<sub>3</sub> + s<sub>3</sub> + ...</code>

**Note:** `a + b` is the concatenation of strings `a` and `b`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/09/02/interleave.jpg)

**Input:** s1 = "aabcc", s2 = "dbbca", s3 = "aadbbcbcac"

**Output:** true

**Explanation:** One way to obtain s3 is: Split s1 into s1 = "aa" + "bc" + "c", and s2 into s2 = "dbbc" + "a". Interleaving the two splits, we get "aa" + "dbbc" + "bc" + "a" + "c" = "aadbbcbcac". Since s3 can be obtained by interleaving s1 and s2, we return true.

**Example 2:**

**Input:** s1 = "aabcc", s2 = "dbbca", s3 = "aadbbbaccc"

**Output:** false

**Explanation:** Notice how it is impossible to interleave s2 with any other string to obtain s3.

**Example 3:**

**Input:** s1 = "", s2 = "", s3 = ""

**Output:** true

**Constraints:**

*   `0 <= s1.length, s2.length <= 100`
*   `0 <= s3.length <= 200`
*   `s1`, `s2`, and `s3` consist of lowercase English letters.

**Follow up:** Could you solve it using only `O(s2.length)` additional memory space?

## Solution

```kotlin
class Solution {
    fun isInterleave(s1: String, s2: String, s3: String): Boolean {
        if (s3.length != s1.length + s2.length) {
            return false
        }
        val cache = Array(s1.length + 1) { arrayOfNulls<Boolean>(s2.length + 1) }
        return isInterleave(s1, s2, s3, 0, 0, 0, cache)
    }

    fun isInterleave(
        s1: String,
        s2: String,
        s3: String,
        i1: Int,
        i2: Int,
        i3: Int,
        cache: Array<Array<Boolean?>>,
    ): Boolean {
        if (cache[i1][i2] != null) {
            return cache[i1][i2]!!
        }
        if (i1 == s1.length && i2 == s2.length && i3 == s3.length) {
            return true
        }
        var result = false
        if (i1 < s1.length && s1[i1] == s3[i3]) {
            result = isInterleave(s1, s2, s3, i1 + 1, i2, i3 + 1, cache)
        }
        if (i2 < s2.length && s2[i2] == s3[i3]) {
            result = result || isInterleave(s1, s2, s3, i1, i2 + 1, i3 + 1, cache)
        }
        cache[i1][i2] = result
        return result
    }
}
```