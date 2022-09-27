[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 60\. Permutation Sequence

Hard

The set `[1, 2, 3, ..., n]` contains a total of `n!` unique permutations.

By listing and labeling all of the permutations in order, we get the following sequence for `n = 3`:

1.  `"123"`
2.  `"132"`
3.  `"213"`
4.  `"231"`
5.  `"312"`
6.  `"321"`

Given `n` and `k`, return the <code>k<sup>th</sup></code> permutation sequence.

**Example 1:**

**Input:** n = 3, k = 3

**Output:** "213"

**Example 2:**

**Input:** n = 4, k = 9

**Output:** "2314"

**Example 3:**

**Input:** n = 3, k = 1

**Output:** "123"

**Constraints:**

*   `1 <= n <= 9`
*   `1 <= k <= n!`

## Solution

```kotlin
@Suppress("NAME_SHADOWING")
class Solution {
    fun getPermutation(n: Int, k: Int): String {
        var k = k
        val res = CharArray(n)
        // We want the permutation sequence to be zero-indexed
        k = k - 1
        // The set bits indicate the available digits
        var a = (1 shl n) - 1
        var m = 1
        for (i in 2 until n) {
            // m = (n - 1)!
            m *= i
        }
        for (i in 0 until n) {
            var b = a
            for (j in 0 until k / m) {
                b = b and b - 1
            }
            // b is the bit corresponding to the digit we want
            b = b and b.inv() + 1
            res[i] = ('1'.code + Integer.bitCount(b - 1)).toChar()
            // Remove b from the set of available digits
            a = a and b.inv()
            k %= m
            m /= Math.max(1, n - i - 1)
        }
        return String(res)
    }
}
```