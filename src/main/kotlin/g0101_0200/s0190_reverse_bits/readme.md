[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 190\. Reverse Bits

Easy

Reverse bits of a given 32 bits unsigned integer.

**Note:**

*   Note that in some languages, such as Java, there is no unsigned integer type. In this case, both input and output will be given as a signed integer type. They should not affect your implementation, as the integer's internal binary representation is the same, whether it is signed or unsigned.
*   In Java, the compiler represents the signed integers using [2's complement notation](https://en.wikipedia.org/wiki/Two%27s_complement). Therefore, in **Example 2** above, the input represents the signed integer `-3` and the output represents the signed integer `-1073741825`.

**Example 1:**

**Input:** n = 00000010100101000001111010011100

**Output:** 964176192 (00111001011110000010100101000000)

**Explanation:** The input binary string **00000010100101000001111010011100** represents the unsigned integer 43261596, so return 964176192 which its binary representation is **00111001011110000010100101000000**. 

**Example 2:**

**Input:** n = 11111111111111111111111111111101

**Output:** 3221225471 (10111111111111111111111111111111)

**Explanation:** The input binary string **11111111111111111111111111111101** represents the unsigned integer 4294967293, so return 3221225471 which its binary representation is **10111111111111111111111111111111**. 

**Constraints:**

*   The input must be a **binary string** of length `32`

**Follow up:** If this function is called many times, how would you optimize it?

## Solution

```kotlin
@Suppress("NAME_SHADOWING")
class Solution {
    // you need treat n as an unsigned value
    fun reverseBits(n: Int): Int {
        var n = n
        var ret = 0
        // because there are 32 bits in total
        for (i in 0..31) {
            ret = ret shl 1
            // If the bit is 1 we OR it with 1, ie add 1
            if (n and 1 > 0) {
                ret = ret or 1
            }
            n = n ushr 1
        }
        return ret
    }
}
```