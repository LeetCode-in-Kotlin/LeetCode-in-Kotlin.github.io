[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1505\. Minimum Possible Integer After at Most K Adjacent Swaps On Digits

Hard

You are given a string `num` representing **the digits** of a very large integer and an integer `k`. You are allowed to swap any two adjacent digits of the integer **at most** `k` times.

Return _the minimum integer you can obtain also as a string_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/06/17/q4_1.jpg)

**Input:** num = "4321", k = 4

**Output:** "1342"

**Explanation:** The steps to obtain the minimum integer from 4321 with 4 adjacent swaps are shown.

**Example 2:**

**Input:** num = "100", k = 1

**Output:** "010"

**Explanation:** It's ok for the output to have leading zeros, but the input is guaranteed not to have any leading zeros.

**Example 3:**

**Input:** num = "36789", k = 1000

**Output:** "36789"

**Explanation:** We can keep the number without any swaps.

**Constraints:**

*   <code>1 <= num.length <= 3 * 10<sup>4</sup></code>
*   `num` consists of only **digits** and does not contain **leading zeros**.
*   <code>1 <= k <= 10<sup>4</sup></code>

## Solution

```kotlin
@Suppress("NAME_SHADOWING")
class Solution {
    fun minInteger(num: String, k: Int): String {
        var k = k
        val sb = StringBuilder()
        val digitPos = IntArray(10)
        val reduceMove = IntArray(10)
        var matchAmount = 0
        val chars = num.toCharArray()
        digitPos.fill(chars.size)
        for (i in chars.indices) {
            val cur = chars[i].code - '0'.code
            if (digitPos[cur] == chars.size) {
                digitPos[cur] = i
                matchAmount++
                if (matchAmount == 10) {
                    break
                }
            }
        }
        var curIndex = 0
        while (k > 0 && curIndex < chars.size) {
            for (digit in 0..9) {
                if (digitPos[digit] < chars.size && digitPos[digit] - reduceMove[digit] <= k) {
                    sb.append(chars[digitPos[digit]])
                    k -= digitPos[digit] - reduceMove[digit]
                    curIndex++
                    reduceMove[digit]++
                    for (j in 0..9) {
                        if (j != digit && digitPos[j] > digitPos[digit]) {
                            reduceMove[j]++
                        }
                    }
                    var find = false
                    for (next in digitPos[digit] + 1 until chars.size) {
                        val cur = chars[next].code - '0'.code
                        if (cur == digit) {
                            find = true
                            digitPos[digit] = next
                            break
                        }
                        if (next < digitPos[cur]) {
                            reduceMove[digit]++
                        }
                    }
                    if (!find) {
                        digitPos[digit] = chars.size
                    }
                    break
                }
            }
        }
        val start = digitPos.min()
        for (i in start until chars.size) {
            if (digitPos[chars[i].code - '0'.code] <= i) {
                sb.append(chars[i])
            }
        }
        return sb.toString()
    }
}
```