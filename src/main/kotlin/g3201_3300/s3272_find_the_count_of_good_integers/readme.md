[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3272\. Find the Count of Good Integers

Hard

You are given two **positive** integers `n` and `k`.

An integer `x` is called **k-palindromic** if:

*   `x` is a palindrome.
*   `x` is divisible by `k`.

An integer is called **good** if its digits can be _rearranged_ to form a **k-palindromic** integer. For example, for `k = 2`, 2020 can be rearranged to form the _k-palindromic_ integer 2002, whereas 1010 cannot be rearranged to form a _k-palindromic_ integer.

Return the count of **good** integers containing `n` digits.

**Note** that _any_ integer must **not** have leading zeros, **neither** before **nor** after rearrangement. For example, 1010 _cannot_ be rearranged to form 101.

**Example 1:**

**Input:** n = 3, k = 5

**Output:** 27

**Explanation:**

_Some_ of the good integers are:

*   551 because it can be rearranged to form 515.
*   525 because it is already k-palindromic.

**Example 2:**

**Input:** n = 1, k = 4

**Output:** 2

**Explanation:**

The two good integers are 4 and 8.

**Example 3:**

**Input:** n = 5, k = 6

**Output:** 2468

**Constraints:**

*   `1 <= n <= 10`
*   `1 <= k <= 9`

## Solution

```kotlin
import kotlin.math.max

class Solution {
    private val palindromes: MutableList<String> = ArrayList()

    private fun factorial(n: Int): Long {
        var res: Long = 1
        for (i in 2..n) {
            res *= i.toLong()
        }
        return res
    }

    private fun countDigits(s: String): MutableMap<Char, Int> {
        val freq: MutableMap<Char, Int> = HashMap()
        for (c in s.toCharArray()) {
            freq[c] = freq.getOrDefault(c, 0) + 1
        }
        return freq
    }

    private fun calculatePermutations(freq: Map<Char, Int>, length: Int): Long {
        var totalPermutations = factorial(length)
        for (count in freq.values) {
            totalPermutations /= factorial(count)
        }
        return totalPermutations
    }

    private fun calculateValidPermutations(s: String): Long {
        val freq = countDigits(s)
        val n = s.length
        var totalPermutations = calculatePermutations(freq, n)
        if (freq.getOrDefault('0', 0) > 0) {
            freq['0'] = freq['0']!! - 1
            val invalidPermutations = calculatePermutations(freq, n - 1)
            totalPermutations -= invalidPermutations
        }
        return totalPermutations
    }

    private fun generatePalindromes(
        f: Int,
        r: Int,
        k: Int,
        lb: Int,
        sum: Int,
        ans: StringBuilder,
        rem: IntArray
    ) {
        if (f > r) {
            if (sum == 0) {
                palindromes.add(ans.toString())
            }
            return
        }
        for (i in lb..9) {
            ans.setCharAt(f, ('0'.code + i).toChar())
            ans.setCharAt(r, ('0'.code + i).toChar())
            var chk = sum
            chk = (chk + rem[f] * i) % k
            if (f != r) {
                chk = (chk + rem[r] * i) % k
            }
            generatePalindromes(f + 1, r - 1, k, 0, chk, ans, rem)
        }
    }

    private fun allKPalindromes(n: Int, k: Int): List<String> {
        val ans = StringBuilder(n)
        ans.append("0".repeat(max(0.0, n.toDouble()).toInt()))
        val rem = IntArray(n)
        rem[0] = 1
        for (i in 1 until n) {
            rem[i] = (rem[i - 1] * 10) % k
        }
        palindromes.clear()
        generatePalindromes(0, n - 1, k, 1, 0, ans, rem)
        return palindromes
    }

    fun countGoodIntegers(n: Int, k: Int): Long {
        val ans = allKPalindromes(n, k)
        val st: MutableSet<String> = HashSet()
        for (str in ans) {
            val arr = str.toCharArray()
            arr.sort()
            st.add(String(arr))
        }
        val v: List<String> = ArrayList(st)
        var chk: Long = 0
        for (str in v) {
            val cc = calculateValidPermutations(str)
            chk += cc
        }
        return chk
    }
}
```