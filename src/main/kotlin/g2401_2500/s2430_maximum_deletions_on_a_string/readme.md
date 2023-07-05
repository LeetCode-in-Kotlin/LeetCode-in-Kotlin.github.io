[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2430\. Maximum Deletions on a String

Hard

You are given a string `s` consisting of only lowercase English letters. In one operation, you can:

*   Delete **the entire string** `s`, or
*   Delete the **first** `i` letters of `s` if the first `i` letters of `s` are **equal** to the following `i` letters in `s`, for any `i` in the range `1 <= i <= s.length / 2`.

For example, if `s = "ababc"`, then in one operation, you could delete the first two letters of `s` to get `"abc"`, since the first two letters of `s` and the following two letters of `s` are both equal to `"ab"`.

Return _the **maximum** number of operations needed to delete all of_ `s`.

**Example 1:**

**Input:** s = "abcabcdabc"

**Output:** 2

**Explanation:** 
- Delete the first 3 letters ("abc") since the next 3 letters are equal. Now, s = "abcdabc". 
 
- Delete all the letters. 

We used 2 operations so return 2. It can be proven that 2 is the maximum number of operations needed. 

Note that in the second operation we cannot delete "abc" again because the next occurrence of "abc" does not happen in the next 3 letters.

**Example 2:**

**Input:** s = "aaabaab"

**Output:** 4

**Explanation:** 
- 
- Delete the first letter ("a") since the next letter is equal. Now, s = "aabaab". 

- Delete the first 3 letters ("aab") since the next 3 letters are equal. Now, s = "aab". 

- Delete the first letter ("a") since the next letter is equal. Now, s = "ab".

- Delete all the letters. 

We used 4 operations so return 4. It can be proven that 4 is the maximum number of operations needed.

**Example 3:**

**Input:** s = "aaaaa"

**Output:** 5

**Explanation:** In each operation, we can delete the first letter of s.

**Constraints:**

*   `1 <= s.length <= 4000`
*   `s` consists only of lowercase English letters.

## Solution

```kotlin
@Suppress("NAME_SHADOWING")
class Solution {
    private var s: String? = null
    private lateinit var hash: IntArray
    private lateinit var pows: IntArray
    private var visited: HashMap<Int, Int>? = null

    fun deleteString(s: String): Int {
        this.s = s
        if (isBad) {
            return s.length
        }
        visited = HashMap()
        fill()
        return helper(0, 1, 0, 1)
    }

    private fun helper(a: Int, b: Int, id1: Int, id2: Int): Int {
        var a = a
        var b = b
        var id2 = id2
        val mask = (id1 shl 12) + id2
        var ans = 1
        if (visited!!.containsKey(mask)) {
            return visited!![mask]!!
        }
        while (b < s!!.length) {
            if ((hash[a + 1] - hash[id1]) * pows[id2] == (hash[b + 1] - hash[id2]) * pows[id1]) {
                ans = if (id2 + 1 == s!!.length) {
                    ans.coerceAtLeast(2)
                } else {
                    ans.coerceAtLeast(1 + helper(id2, id2 + 1, id2, id2 + 1))
                }
            }
            a++
            id2++
            b += 2
        }
        visited!![mask] = ans
        return ans
    }

    private fun fill() {
        val n = s!!.length
        hash = IntArray(n + 1)
        pows = IntArray(n)
        pows[0] = 1
        hash[1] = s!![0].code
        for (i in 1 until n) {
            pows[i] = pows[i - 1] * 1000000007
            val temp = pows[i]
            hash[i + 1] = s!![i].code * temp + hash[i]
        }
    }

    private val isBad: Boolean
        get() {
            var i = 1
            while (i < s!!.length) {
                if (s!![0] != s!![i++]) {
                    return false
                }
            }
            return true
        }
}
```