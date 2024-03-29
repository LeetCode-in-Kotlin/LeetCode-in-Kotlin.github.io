[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1625\. Lexicographically Smallest String After Applying Operations

Medium

You are given a string `s` of **even length** consisting of digits from `0` to `9`, and two integers `a` and `b`.

You can apply either of the following two operations any number of times and in any order on `s`:

*   Add `a` to all odd indices of `s` **(0-indexed)**. Digits post `9` are cycled back to `0`. For example, if `s = "3456"` and `a = 5`, `s` becomes `"3951"`.
*   Rotate `s` to the right by `b` positions. For example, if `s = "3456"` and `b = 1`, `s` becomes `"6345"`.

Return _the **lexicographically smallest** string you can obtain by applying the above operations any number of times on_ `s`.

A string `a` is lexicographically smaller than a string `b` (of the same length) if in the first position where `a` and `b` differ, string `a` has a letter that appears earlier in the alphabet than the corresponding letter in `b`. For example, `"0158"` is lexicographically smaller than `"0190"` because the first position they differ is at the third letter, and `'5'` comes before `'9'`.

**Example 1:**

**Input:** s = "5525", a = 9, b = 2

**Output:** "2050"

**Explanation:** We can apply the following operations: 

Start: "5525" 

Rotate: "2555" 

Add: "2454" 

Add: "2353" 

Rotate: "5323" 

Add: "5222" 

Add: "5121" 

Rotate: "2151" 

Add: "2050" 

There is no way to obtain a string that is lexicographically smaller then "2050".

**Example 2:**

**Input:** s = "74", a = 5, b = 1

**Output:** "24"

**Explanation:** We can apply the following operations:

Start: "74" 

Rotate: "47"

Add: "42"

Rotate: "24"

There is no way to obtain a string that is lexicographically smaller then "24".

**Example 3:**

**Input:** s = "0011", a = 4, b = 2

**Output:** "0011"

**Explanation:** There are no sequence of operations that will give us a lexicographically smaller string than "0011".

**Constraints:**

*   `2 <= s.length <= 100`
*   `s.length` is even.
*   `s` consists of digits from `0` to `9` only.
*   `1 <= a <= 9`
*   `1 <= b <= s.length - 1`

## Solution

```kotlin
@Suppress("NAME_SHADOWING")
class Solution {
    private var ans = "z"

    private fun dfs(s: String, a: Int, b: Int, set: HashSet<String>) {
        if (set.contains(s)) {
            return
        }
        set.add(s)
        val one = add(s, a)
        val two = rotate(s, b)
        dfs(one, a, b, set)
        dfs(two, a, b, set)
    }

    private fun add(s: String, a: Int): String {
        var s = s
        val temp = s.toCharArray()
        var i = 1
        while (i < temp.size) {
            var `val` = temp[i].code - '0'.code
            `val` = (`val` + a) % 10
            temp[i] = (`val` + '0'.code).toChar()
            i += 2
        }
        s = String(temp)
        if (ans > s) {
            ans = s
        }
        return s
    }

    private fun rotate(s: String, b: Int): String {
        var s = s
        var b = b
        if (b < 0) {
            b += s.length
        }
        b %= s.length
        b = s.length - b
        s = s.substring(b) + s.substring(0, b)
        if (ans > s) {
            ans = s
        }
        return s
    }

    fun findLexSmallestString(s: String, a: Int, b: Int): String {
        val set = HashSet<String>()
        dfs(s, a, b, set)
        return ans
    }
}
```