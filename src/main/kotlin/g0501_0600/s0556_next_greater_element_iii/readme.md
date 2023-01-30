[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 556\. Next Greater Element III

Medium

Given a positive integer `n`, find _the smallest integer which has exactly the same digits existing in the integer_ `n` _and is greater in value than_ `n`. If no such positive integer exists, return `-1`.

**Note** that the returned integer should fit in **32-bit integer**, if there is a valid answer but it does not fit in **32-bit integer**, return `-1`.

**Example 1:**

**Input:** n = 12

**Output:** 21

**Example 2:**

**Input:** n = 21

**Output:** -1

**Constraints:**

*   <code>1 <= n <= 2<sup>31</sup> - 1</code>

## Solution

```kotlin
@Suppress("NAME_SHADOWING")
class Solution {
    /*
    - What this problem wants is finding the next permutation of n
    - Steps to find the next permuation:
       find largest index k such that inp[k] < inp[k+1];
           if k == -1: return -1
           else:
               look for largest index l such that inp[l] > inp[k]
               swap the two index
               reverse from k+1 to n.length
       */
    fun nextGreaterElement(n: Int): Int {
        val inp = n.toString().toCharArray()
        // Find k
        var k = -1
        for (i in inp.size - 2 downTo 0) {
            if (inp[i] < inp[i + 1]) {
                k = i
                break
            }
        }
        if (k == -1) {
            return -1
        }
        // Find l
        var largerIdx = inp.size - 1
        for (i in inp.indices.reversed()) {
            if (inp[i] > inp[k]) {
                largerIdx = i
                break
            }
        }
        swap(inp, k, largerIdx)
        reverse(inp, k + 1, inp.size - 1)
        // Build result
        var ret = 0
        for (c in inp) {
            val digit = c.code - '0'.code
            // Handle the case if ret > Integer.MAX_VALUE - This idea is borrowed from problem  8.
            // String to Integer (atoi)
            if (ret > Int.MAX_VALUE / 10 || ret == Int.MAX_VALUE / 10 && digit > Int.MAX_VALUE % 10) {
                return -1
            }
            ret = ret * 10 + (c.code - '0'.code)
        }
        return ret
    }

    private fun swap(inp: CharArray, i: Int, j: Int) {
        val temp = inp[i]
        inp[i] = inp[j]
        inp[j] = temp
    }

    private fun reverse(inp: CharArray, start: Int, end: Int) {
        var start = start
        var end = end
        while (start < end) {
            val temp = inp[start]
            inp[start] = inp[end]
            inp[end] = temp
            start++
            end--
        }
    }
}
```