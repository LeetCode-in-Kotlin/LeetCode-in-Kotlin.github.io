[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 806\. Number of Lines To Write String

Easy

You are given a string `s` of lowercase English letters and an array `widths` denoting **how many pixels wide** each lowercase English letter is. Specifically, `widths[0]` is the width of `'a'`, `widths[1]` is the width of `'b'`, and so on.

You are trying to write `s` across several lines, where **each line is no longer than** `100` **pixels**. Starting at the beginning of `s`, write as many letters on the first line such that the total width does not exceed `100` pixels. Then, from where you stopped in `s`, continue writing as many letters as you can on the second line. Continue this process until you have written all of `s`.

Return _an array_ `result` _of length 2 where:_

*   `result[0]` _is the total number of lines._
*   `result[1]` _is the width of the last line in pixels._

**Example 1:**

**Input:** widths = [10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10], s = "abcdefghijklmnopqrstuvwxyz"

**Output:** [3,60]

**Explanation:** You can write s as follows:

abcdefghij // 100 pixels wide

klmnopqrst // 100 pixels wide

uvwxyz // 60 pixels wide

There are a total of 3 lines, and the last line is 60 pixels wide.

**Example 2:**

**Input:** widths = [4,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10], s = "bbbcccdddaaa"

**Output:** [2,4]

**Explanation:** You can write s as follows:

bbbcccdddaa // 98 pixels wide

a // 4 pixels wide

There are a total of 2 lines, and the last line is 4 pixels wide.

**Constraints:**

*   `widths.length == 26`
*   `2 <= widths[i] <= 10`
*   `1 <= s.length <= 1000`
*   `s` contains only lowercase English letters.

## Solution

```kotlin
class Solution {
    fun numberOfLines(widths: IntArray, s: String): IntArray {
        var count = 0
        var line = 0
        var i = 0
        while (i < s.length) {
            count += widths[s[i].code - 'a'.code]
            if (count == 100) {
                line++
                count = 0
            }
            if (count > 100) {
                line++
                i--
                count = 0
            }
            i++
        }
        if (count in 1..99) {
            line++
        }
        if (count == 0) {
            count = 100
        }
        return intArrayOf(line, count)
    }
}
```