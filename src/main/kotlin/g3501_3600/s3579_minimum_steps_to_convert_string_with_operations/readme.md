[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3579\. Minimum Steps to Convert String with Operations

Hard

You are given two strings, `word1` and `word2`, of equal length. You need to transform `word1` into `word2`.

For this, divide `word1` into one or more **contiguous **substring****. For each substring `substr` you can perform the following operations:

1.  **Replace:** Replace the character at any one index of `substr` with another lowercase English letter.
    
2.  **Swap:** Swap any two characters in `substr`.
    
3.  **Reverse Substring:** Reverse `substr`.
    

Each of these counts as **one** operation and each character of each substring can be used in each type of operation at most once (i.e. no single index may be involved in more than one replace, one swap, or one reverse).

Return the **minimum number of operations** required to transform `word1` into `word2`.

**Example 1:**

**Input:** word1 = "abcdf", word2 = "dacbe"

**Output:** 4

**Explanation:**

Divide `word1` into `"ab"`, `"c"`, and `"df"`. The operations are:

*   For the substring `"ab"`,
    *   Perform operation of type 3 on `"ab" -> "ba"`.
    *   Perform operation of type 1 on `"ba" -> "da"`.
*   For the substring `"c"` do no operations.
*   For the substring `"df"`,
    *   Perform operation of type 1 on `"df" -> "bf"`.
    *   Perform operation of type 1 on `"bf" -> "be"`.

**Example 2:**

**Input:** word1 = "abceded", word2 = "baecfef"

**Output:** 4

**Explanation:**

Divide `word1` into `"ab"`, `"ce"`, and `"ded"`. The operations are:

*   For the substring `"ab"`,
    *   Perform operation of type 2 on `"ab" -> "ba"`.
*   For the substring `"ce"`,
    *   Perform operation of type 2 on `"ce" -> "ec"`.
*   For the substring `"ded"`,
    *   Perform operation of type 1 on `"ded" -> "fed"`.
    *   Perform operation of type 1 on `"fed" -> "fef"`.

**Example 3:**

**Input:** word1 = "abcdef", word2 = "fedabc"

**Output:** 2

**Explanation:**

Divide `word1` into `"abcdef"`. The operations are:

*   For the substring `"abcdef"`,
    *   Perform operation of type 3 on `"abcdef" -> "fedcba"`.
    *   Perform operation of type 2 on `"fedcba" -> "fedabc"`.

**Constraints:**

*   `1 <= word1.length == word2.length <= 100`
*   `word1` and `word2` consist only of lowercase English letters.

## Solution

```kotlin
import kotlin.math.min

class Solution {
    fun minOperations(word1: String, word2: String): Int {
        val dp = IntArray(word1.length)
        val count: Array<IntArray> = Array<IntArray>(26) { IntArray(26) }
        for (i in 0..<word1.length) {
            dp[i] = Int.Companion.MAX_VALUE
        }
        for (i in 0..<word1.length) {
            for (j in i downTo 0) {
                var c1 = 0
                var c2 = 0
                run {
                    var k1 = j
                    var k2 = j
                    while (k1 <= i && k2 <= i) {
                        val ints = count[word2[k2].code - 'a'.code]
                        if (ints[word1[k1].code - 'a'.code] > 0) {
                            ints[word1[k1].code - 'a'.code]--
                        } else if (word1[k1] != word2[k2]) {
                            count[word1[k1].code - 'a'.code][word2[k2].code - 'a'.code]++
                            c1++
                        }
                        k1++
                        k2++
                    }
                }
                run {
                    var k1 = j
                    var k2 = j
                    while (k1 <= i && k2 <= i) {
                        count[word1[k1].code - 'a'.code][word2[k2].code - 'a'.code] = 0
                        k1++
                        k2++
                    }
                }
                dp[i] = min(dp[i], if (j - 1 < 0) c1 else dp[j - 1] + c1)
                run {
                    var k1 = j
                    var k2 = i
                    while (k1 <= i && k2 >= j) {
                        val ints = count[word2[k2].code - 'a'.code]
                        if (ints[word1[k1].code - 'a'.code] > 0) {
                            ints[word1[k1].code - 'a'.code]--
                        } else if (word1[k1].code - 'a'.code != word2[k2].code - 'a'.code) {
                            count[word1[k1].code - 'a'.code][word2[k2].code - 'a'.code]++
                            c2++
                        }
                        k1++
                        k2--
                    }
                }
                var k1 = j
                var k2 = i
                while (k1 <= i && k2 >= j) {
                    count[word1[k1].code - 'a'.code][word2[k2].code - 'a'.code] = 0
                    k1++
                    k2--
                }
                dp[i] = min(dp[i], if (j - 1 < 0) c2 + 1 else dp[j - 1] + c2 + 1)
            }
        }
        return dp[word1.length - 1]
    }
}
```