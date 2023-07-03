[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2381\. Shifting Letters II

Medium

You are given a string `s` of lowercase English letters and a 2D integer array `shifts` where <code>shifts[i] = [start<sub>i</sub>, end<sub>i</sub>, direction<sub>i</sub>]</code>. For every `i`, **shift** the characters in `s` from the index <code>start<sub>i</sub></code> to the index <code>end<sub>i</sub></code> (**inclusive**) forward if <code>direction<sub>i</sub> = 1</code>, or shift the characters backward if <code>direction<sub>i</sub> = 0</code>.

Shifting a character **forward** means replacing it with the **next** letter in the alphabet (wrapping around so that `'z'` becomes `'a'`). Similarly, shifting a character **backward** means replacing it with the **previous** letter in the alphabet (wrapping around so that `'a'` becomes `'z'`).

Return _the final string after all such shifts to_ `s` _are applied_.

**Example 1:**

**Input:** s = "abc", shifts = \[\[0,1,0],[1,2,1],[0,2,1]]

**Output:** "ace"

**Explanation:** Firstly, shift the characters from index 0 to index 1 backward. Now s = "zac".

Secondly, shift the characters from index 1 to index 2 forward. Now s = "zbd".

Finally, shift the characters from index 0 to index 2 forward. Now s = "ace".

**Example 2:**

**Input:** s = "dztz", shifts = \[\[0,0,0],[1,1,1]]

**Output:** "catz"

**Explanation:** Firstly, shift the characters from index 0 to index 0 backward. Now s = "cztz".

Finally, shift the characters from index 1 to index 1 forward. Now s = "catz".

**Constraints:**

*   <code>1 <= s.length, shifts.length <= 5 * 10<sup>4</sup></code>
*   `shifts[i].length == 3`
*   <code>0 <= start<sub>i</sub> <= end<sub>i</sub> < s.length</code>
*   <code>0 <= direction<sub>i</sub> <= 1</code>
*   `s` consists of lowercase English letters.

## Solution

```kotlin
class Solution {
    fun shiftingLetters(s: String, shifts: Array<IntArray>): String {
        val diff = IntArray(s.length + 1)
        var l: Int
        var r: Int
        for (shift in shifts) {
            l = shift[0]
            r = shift[1] + 1
            diff[l] += 26
            diff[r] += 26
            if (shift[2] == 0) {
                diff[l]--
                diff[r]++
            } else {
                diff[l]++
                diff[r]--
            }
            diff[l] %= 26
            diff[r] %= 26
        }
        val sb = StringBuilder()
        var current = 0
        var `val`: Int
        for (i in 0 until s.length) {
            current += diff[i]
            `val` = s[i].code - 'a'.code
            `val` += current
            `val` %= 26
            sb.append(('a'.code + `val`).toChar())
        }
        return sb.toString()
    }
}
```