[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1320\. Minimum Distance to Type a Word Using Two Fingers

Hard

![](https://assets.leetcode.com/uploads/2020/01/02/leetcode_keyboard.png)

You have a keyboard layout as shown above in the **X-Y** plane, where each English uppercase letter is located at some coordinate.

*   For example, the letter `'A'` is located at coordinate `(0, 0)`, the letter `'B'` is located at coordinate `(0, 1)`, the letter `'P'` is located at coordinate `(2, 3)` and the letter `'Z'` is located at coordinate `(4, 1)`.

Given the string `word`, return _the minimum total **distance** to type such string using only two fingers_.

The **distance** between coordinates <code>(x<sub>1</sub>, y<sub>1</sub>)</code> and <code>(x<sub>2</sub>, y<sub>2</sub>)</code> is <code>|x<sub>1</sub> - x<sub>2</sub>| + |y<sub>1</sub> - y<sub>2</sub>|</code>.

**Note** that the initial positions of your two fingers are considered free so do not count towards your total distance, also your two fingers do not have to start at the first letter or the first two letters.

**Example 1:**

**Input:** word = "CAKE"

**Output:** 3

**Explanation:** Using two fingers, one optimal way to type "CAKE" is: 

Finger 1 on letter 'C' -> cost = 0 

Finger 1 on letter 'A' -> cost = Distance from letter 'C' to letter 'A' = 2 

Finger 2 on letter 'K' -> cost = 0 

Finger 2 on letter 'E' -> cost = Distance from letter 'K' to letter 'E' = 1 

Total distance = 3

**Example 2:**

**Input:** word = "HAPPY"

**Output:** 6

**Explanation:** Using two fingers, one optimal way to type "HAPPY" is: 

Finger 1 on letter 'H' -> cost = 0 

Finger 1 on letter 'A' -> cost = Distance from letter 'H' to letter 'A' = 2 

Finger 2 on letter 'P' -> cost = 0 

Finger 2 on letter 'P' -> cost = Distance from letter 'P' to letter 'P' = 0 

Finger 1 on letter 'Y' -> cost = Distance from letter 'A' to letter 'Y' = 4 

Total distance = 6

**Constraints:**

*   `2 <= word.length <= 300`
*   `word` consists of uppercase English letters.

## Solution

```kotlin
class Solution {
    private var word: String? = null
    private lateinit var dp: Array<Array<Array<Int?>>>

    fun minimumDistance(word: String): Int {
        this.word = word
        dp = Array(27) { Array(27) { arrayOfNulls(word.length) } }
        return find(null, null, 0)
    }

    private fun find(f1: Char?, f2: Char?, index: Int): Int {
        if (index == word!!.length) {
            return 0
        }
        val result = dp[if (f1 == null) 0 else f1.code - 'A'.code + 1][
            if (f2 == null) 0 else f2.code - 'A'.code + 1
        ][index]
        if (result != null) {
            return result
        }
        val ic = word!![index]
        var move = move(f1, ic) + find(ic, f2, index + 1)
        move = Math.min(move, move(f2, ic) + find(f1, ic, index + 1))
        dp[if (f1 == null) 0 else f1.code - 'A'.code + 1][if (f2 == null) 0 else f2.code - 'A'.code + 1][index] = move
        return move
    }

    private fun move(c1: Char?, c2: Char): Int {
        if (c1 == null) {
            return 0
        }
        val c1x = (c1.code - 'A'.code) % 6
        val c1y = (c1.code - 'A'.code) / 6
        val c2x = (c2.code - 'A'.code) % 6
        val c2y = (c2.code - 'A'.code) / 6
        return Math.abs(c1x - c2x) + Math.abs(c1y - c2y)
    }
}
```