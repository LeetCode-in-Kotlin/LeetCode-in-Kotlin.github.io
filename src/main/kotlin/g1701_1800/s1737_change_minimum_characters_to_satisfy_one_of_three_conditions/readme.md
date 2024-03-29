[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1737\. Change Minimum Characters to Satisfy One of Three Conditions

Medium

You are given two strings `a` and `b` that consist of lowercase letters. In one operation, you can change any character in `a` or `b` to **any lowercase letter**.

Your goal is to satisfy **one** of the following three conditions:

*   **Every** letter in `a` is **strictly less** than **every** letter in `b` in the alphabet.
*   **Every** letter in `b` is **strictly less** than **every** letter in `a` in the alphabet.
*   **Both** `a` and `b` consist of **only one** distinct letter.

Return _the **minimum** number of operations needed to achieve your goal._

**Example 1:**

**Input:** a = "aba", b = "caa"

**Output:** 2

**Explanation:** Consider the best way to make each condition true: 

1) Change b to "ccc" in 2 operations, then every letter in a is less than every letter in b. 

2) Change a to "bbb" and b to "aaa" in 3 operations, then every letter in b is less than every letter in a. 

3) Change a to "aaa" and b to "aaa" in 2 operations, then a and b consist of one distinct letter. The best way was done in 2 operations (either condition 1 or condition 3).

**Example 2:**

**Input:** a = "dabadd", b = "cda"

**Output:** 3

**Explanation:** The best way is to make condition 1 true by changing b to "eee".

**Constraints:**

*   <code>1 <= a.length, b.length <= 10<sup>5</sup></code>
*   `a` and `b` consist only of lowercase letters.

## Solution

```kotlin
class Solution {
    fun minCharacters(a: String, b: String): Int {
        val array1 = IntArray(26)
        val array2 = IntArray(26)
        val l1: Int = a.length
        val l2: Int = b.length
        for (i: Char in a.toCharArray()) {
            array1[i.code - 'a'.code]++
        }
        for (i: Char in b.toCharArray()) {
            array2[i.code - 'a'.code]++
        }
        var min: Int = Int.MAX_VALUE
        var t1 = 0
        var t2 = 0
        var max: Int = -1
        for (i in 0..24) {
            t1 += array1[i]
            t2 += array2[i]
            min = Math.min(min, Math.min(t1 + l2 - t2, t2 + l1 - t1))
            max = Math.max(max, array1[i] + array2[i])
        }
        max = Math.max(max, array1[25] + array2[25])
        return Math.min(min, l1 + l2 - max)
    }
}
```