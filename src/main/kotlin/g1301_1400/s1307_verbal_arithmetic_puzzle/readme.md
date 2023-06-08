[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1307\. Verbal Arithmetic Puzzle

Hard

Given an equation, represented by `words` on the left side and the `result` on the right side.

You need to check if the equation is solvable under the following rules:

*   Each character is decoded as one digit (0 - 9).
*   Every pair of different characters must map to different digits.
*   Each `words[i]` and `result` are decoded as one number **without** leading zeros.
*   Sum of numbers on the left side (`words`) will equal to the number on the right side (`result`).

Return `true` _if the equation is solvable, otherwise return_ `false`.

**Example 1:**

**Input:** words = ["SEND","MORE"], result = "MONEY"

**Output:** true

**Explanation:** Map 'S'-> 9, 'E'->5, 'N'->6, 'D'->7, 'M'->1, 'O'->0, 'R'->8, 'Y'->'2' Such that: "SEND" + "MORE" = "MONEY" , 9567 + 1085 = 10652

**Example 2:**

**Input:** words = ["SIX","SEVEN","SEVEN"], result = "TWENTY"

**Output:** true

**Explanation:** Map 'S'-> 6, 'I'->5, 'X'->0, 'E'->8, 'V'->7, 'N'->2, 'T'->1, 'W'->'3', 'Y'->4 Such that: "SIX" + "SEVEN" + "SEVEN" = "TWENTY" , 650 + 68782 + 68782 = 138214

**Example 3:**

**Input:** words = ["LEET","CODE"], result = "POINT"

**Output:** false

**Constraints:**

*   `2 <= words.length <= 5`
*   `1 <= words[i].length, result.length <= 7`
*   `words[i], result` contain only uppercase English letters.
*   The number of different characters used in the expression is at most `10`.

## Solution

```kotlin
class Solution {
    private lateinit var map: IntArray
    private lateinit var grid: Array<CharArray>
    private var solved = false
    private lateinit var usedDigit: BooleanArray
    private lateinit var mustNotBeZero: BooleanArray
    private var cols = 0
    private var resultRow = 0

    fun isSolvable(words: Array<String>, result: String): Boolean {
        solved = false
        val rows = words.size + 1
        cols = result.length
        grid = Array(rows) { CharArray(cols) }
        mustNotBeZero = BooleanArray(26)
        usedDigit = BooleanArray(10)
        resultRow = rows - 1
        map = IntArray(26)
        map.fill(-1)
        var maxLength = 0
        for (i in words.indices) {
            var j = words[i].length
            if (j > maxLength) {
                maxLength = j
            }
            if (j > 1) {
                mustNotBeZero[words[i][0].code - 'A'.code] = true
            }
            if (j > cols) {
                return false
            }
            for (c in words[i].toCharArray()) {
                grid[i][--j] = c
            }
        }
        if (maxLength + 1 < cols) {
            return false
        }
        var j = cols
        if (j > 1) {
            mustNotBeZero[result[0].code - 'A'.code] = true
        }
        for (c in result.toCharArray()) {
            grid[resultRow][--j] = c
        }
        backtrack(0, 0, 0)
        return solved
    }

    private fun canPlace(ci: Int, d: Int): Boolean {
        return !usedDigit[d] && map[ci] == -1 || map[ci] == d
    }

    private fun placeNum(ci: Int, d: Int) {
        usedDigit[d] = true
        map[ci] = d
    }

    private fun removeNum(ci: Int, d: Int) {
        usedDigit[d] = false
        map[ci] = -1
    }

    private fun placeNextNum(r: Int, c: Int, sum: Int) {
        if (r == resultRow && c == cols - 1) {
            solved = sum == 0
        } else {
            if (r == resultRow) {
                backtrack(0, c + 1, sum)
            } else {
                backtrack(r + 1, c, sum)
            }
        }
    }

    private fun backtrack(r: Int, c: Int, sum: Int) {
        val unused = '\u0000'
        if (grid[r][c] == unused) {
            placeNextNum(r, c, sum)
        } else {
            val ci = grid[r][c].code - 'A'.code
            if (r == resultRow) {
                val d = sum % 10
                if (map[ci] == -1) {
                    if (canPlace(ci, d)) {
                        placeNum(ci, d)
                        placeNextNum(r, c, sum / 10)
                        if (solved) {
                            return
                        }
                        removeNum(ci, d)
                    }
                } else {
                    if (map[ci] == d) {
                        placeNextNum(r, c, sum / 10)
                    }
                }
            } else {
                if (map[ci] == -1) {
                    val startIndex = if (mustNotBeZero[ci]) 1 else 0
                    for (d in startIndex..9) {
                        if (canPlace(ci, d)) {
                            placeNum(ci, d)
                            placeNextNum(r, c, sum + d)
                            if (solved) {
                                return
                            }
                            removeNum(ci, d)
                        }
                    }
                } else {
                    placeNextNum(r, c, sum + map[ci])
                }
            }
        }
    }
}
```