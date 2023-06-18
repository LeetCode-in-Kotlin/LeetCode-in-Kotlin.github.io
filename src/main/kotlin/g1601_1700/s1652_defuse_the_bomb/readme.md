[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1652\. Defuse the Bomb

Easy

You have a bomb to defuse, and your time is running out! Your informer will provide you with a **circular** array `code` of length of `n` and a key `k`.

To decrypt the code, you must replace every number. All the numbers are replaced **simultaneously**.

*   If `k > 0`, replace the <code>i<sup>th</sup></code> number with the sum of the **next** `k` numbers.
*   If `k < 0`, replace the <code>i<sup>th</sup></code> number with the sum of the **previous** `k` numbers.
*   If `k == 0`, replace the <code>i<sup>th</sup></code> number with `0`.

As `code` is circular, the next element of `code[n-1]` is `code[0]`, and the previous element of `code[0]` is `code[n-1]`.

Given the **circular** array `code` and an integer key `k`, return _the decrypted code to defuse the bomb_!

**Example 1:**

**Input:** code = [5,7,1,4], k = 3

**Output:** [12,10,16,13]

**Explanation:** Each number is replaced by the sum of the next 3 numbers. The decrypted code is [7+1+4, 1+4+5, 4+5+7, 5+7+1]. Notice that the numbers wrap around.

**Example 2:**

**Input:** code = [1,2,3,4], k = 0

**Output:** [0,0,0,0]

**Explanation:** When k is zero, the numbers are replaced by 0.

**Example 3:**

**Input:** code = [2,4,9,3], k = -2

**Output:** [12,5,6,13]

**Explanation:** The decrypted code is [3+9, 2+3, 4+2, 9+4]. Notice that the numbers wrap around again. If k is negative, the sum is of the **previous** numbers.

**Constraints:**

*   `n == code.length`
*   `1 <= n <= 100`
*   `1 <= code[i] <= 100`
*   `-(n - 1) <= k <= n - 1`

## Solution

```kotlin
class Solution {
    fun decrypt(code: IntArray, k: Int): IntArray {
        val result = IntArray(code.size)
        val len = code.size
        if (k == 0) {
            for (i in code.indices) {
                result[i] = 0
            }
        } else if (k > 0) {
            var kSum = 0
            for (i in 1..k) {
                kSum += code[i]
            }
            result[0] = kSum
            for (i in 1 until len) {
                kSum -= code[i]
                kSum += code[(i + k) % len]
                result[i] = kSum
            }
        } else {
            var kSum = 0
            val kVal = Math.abs(k)
            for (i in len - 1 downTo len - kVal) {
                kSum += code[i]
            }
            result[0] = kSum
            for (i in 1 until len) {
                kSum -= code[(len - kVal + i - 1) % len]
                kSum += code[i - 1]
                result[i] = kSum
            }
        }
        return result
    }
}
```