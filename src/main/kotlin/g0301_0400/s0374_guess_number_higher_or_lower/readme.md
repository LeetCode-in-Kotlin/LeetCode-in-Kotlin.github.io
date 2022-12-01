[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 374\. Guess Number Higher or Lower

Easy

We are playing the Guess Game. The game is as follows:

I pick a number from `1` to `n`. You have to guess which number I picked.

Every time you guess wrong, I will tell you whether the number I picked is higher or lower than your guess.

You call a pre-defined API `int guess(int num)`, which returns three possible results:

*   `-1`: Your guess is higher than the number I picked (i.e. `num > pick`).
*   `1`: Your guess is lower than the number I picked (i.e. `num < pick`).
*   `0`: your guess is equal to the number I picked (i.e. `num == pick`).

Return _the number that I picked_.

**Example 1:**

**Input:** n = 10, pick = 6

**Output:** 6

**Example 2:**

**Input:** n = 1, pick = 1

**Output:** 1

**Example 3:**

**Input:** n = 2, pick = 1

**Output:** 1

**Constraints:**

*   <code>1 <= n <= 2<sup>31</sup> - 1</code>
*   `1 <= pick <= n`

## Solution

```kotlin
/*
 * The API guess is defined in the parent class.
 * @param num   your guess
 * @return -1 if num is higher than the picked number
 *			      1 if num is lower than the picked number
 *               otherwise return 0
 * fun guess(num:Int):Int {}
 */

class Solution : GuessGame() {
    fun guessNumber(n: Int): Int {
        var start = 0
        var end = n
        while (start <= end) {
            val mid = start + (end - start) / 2
            if (guess(mid) == 0) {
                return mid
            } else if (guess(mid) == -1) {
                end = mid - 1
            } else {
                start = mid + 1
            }
        }
        return -1
    }
}
```