[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 816\. Ambiguous Coordinates

Medium

We had some 2-dimensional coordinates, like `"(1, 3)"` or `"(2, 0.5)"`. Then, we removed all commas, decimal points, and spaces and ended up with the string s.

*   For example, `"(1, 3)"` becomes `s = "(13)"` and `"(2, 0.5)"` becomes `s = "(205)"`.

Return _a list of strings representing all possibilities for what our original coordinates could have been_.

Our original representation never had extraneous zeroes, so we never started with numbers like `"00"`, `"0.0"`, `"0.00"`, `"1.0"`, `"001"`, `"00.01"`, or any other number that can be represented with fewer digits. Also, a decimal point within a number never occurs without at least one digit occurring before it, so we never started with numbers like `".1"`.

The final answer list can be returned in any order. All coordinates in the final answer have exactly one space between them (occurring after the comma.)

**Example 1:**

**Input:** s = "(123)"

**Output:** ["(1, 2.3)","(1, 23)","(1.2, 3)","(12, 3)"]

**Example 2:**

**Input:** s = "(0123)"

**Output:** ["(0, 1.23)","(0, 12.3)","(0, 123)","(0.1, 2.3)","(0.1, 23)","(0.12, 3)"]

**Explanation:** 0.0, 00, 0001 or 00.01 are not allowed.

**Example 3:**

**Input:** s = "(00011)"

**Output:** ["(0, 0.011)","(0.001, 1)"]

**Constraints:**

*   `4 <= s.length <= 12`
*   `s[0] == '('` and `s[s.length - 1] == ')'`.
*   The rest of `s` are digits.

## Solution

```kotlin
@Suppress("kotlin:S107")
class Solution {
    fun ambiguousCoordinates(s: String): List<String> {
        val sc = s.toCharArray()
        val result: MutableList<String> = ArrayList()
        val sb = StringBuilder()
        for (commaPos in 2 until sc.size - 1) {
            if (isValidNum(sc, 1, commaPos - 1)) {
                if (isValidNum(sc, commaPos, sc.size - 2)) {
                    buildNumbs(result, sb, sc, commaPos - 1, 0, commaPos, sc.size - 2, 0)
                }
                for (dp2Idx in commaPos + 1 until sc.size - 1) {
                    if (isValidDPNum(sc, commaPos, sc.size - 2, dp2Idx)) {
                        buildNumbs(result, sb, sc, commaPos - 1, 0, commaPos, sc.size - 2, dp2Idx)
                    }
                }
            }
            for (dp1Idx in 2 until commaPos) {
                if (isValidDPNum(sc, 1, commaPos - 1, dp1Idx)) {
                    if (isValidNum(sc, commaPos, sc.size - 2)) {
                        buildNumbs(result, sb, sc, commaPos - 1, dp1Idx, commaPos, sc.size - 2, 0)
                    }
                    for (dp2Idx in commaPos + 1 until sc.size - 1) {
                        if (isValidDPNum(sc, commaPos, sc.size - 2, dp2Idx)) {
                            buildNumbs(
                                result,
                                sb,
                                sc,
                                commaPos - 1,
                                dp1Idx,
                                commaPos,
                                sc.size - 2,
                                dp2Idx
                            )
                        }
                    }
                }
            }
        }
        return result
    }

    private fun isValidNum(sc: CharArray, startIdx: Int, lastIdx: Int): Boolean {
        return sc[startIdx] != '0' || lastIdx - startIdx == 0
    }

    private fun isValidDPNum(sc: CharArray, startIdx: Int, lastIdx: Int, dpIdx: Int): Boolean {
        return (sc[startIdx] != '0' || dpIdx - startIdx == 1) && sc[lastIdx] != '0'
    }

    private fun buildNumbs(
        result: MutableList<String>,
        sb: StringBuilder,
        sc: CharArray,
        last1Idx: Int,
        dp1Idx: Int,
        start2Idx: Int,
        last2Idx: Int,
        dp2Idx: Int
    ) {
        sb.setLength(0)
        sb.append('(')
        if (dp1Idx == 0) {
            sb.append(sc, 1, last1Idx - 1 + 1)
        } else {
            sb.append(sc, 1, dp1Idx - 1).append('.').append(sc, dp1Idx, last1Idx - dp1Idx + 1)
        }
        sb.append(',').append(' ')
        if (dp2Idx == 0) {
            sb.append(sc, start2Idx, last2Idx - start2Idx + 1)
        } else {
            sb.append(sc, start2Idx, dp2Idx - start2Idx)
                .append('.')
                .append(sc, dp2Idx, last2Idx - dp2Idx + 1)
        }
        sb.append(')')
        result.add(sb.toString())
    }
}
```