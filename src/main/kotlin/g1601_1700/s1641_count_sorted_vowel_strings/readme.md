[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1641\. Count Sorted Vowel Strings

Medium

Given an integer `n`, return _the number of strings of length_ `n` _that consist only of vowels (_`a`_,_ `e`_,_ `i`_,_ `o`_,_ `u`_) and are **lexicographically sorted**._

A string `s` is **lexicographically sorted** if for all valid `i`, `s[i]` is the same as or comes before `s[i+1]` in the alphabet.

**Example 1:**

**Input:** n = 1

**Output:** 5

**Explanation:** The 5 sorted strings that consist of vowels only are `["a","e","i","o","u"].`

**Example 2:**

**Input:** n = 2

**Output:** 15

**Explanation:** The 15 sorted strings that consist of vowels only are 

["aa","ae","ai","ao","au","ee","ei","eo","eu","ii","io","iu","oo","ou","uu"]. 

Note that "ea" is not a valid string since 'e' comes after 'a' in the alphabet.

**Example 3:**

**Input:** n = 33

**Output:** 66045

**Constraints:**

*   `1 <= n <= 50`

## Solution

```kotlin
class Solution {
    fun countVowelStrings(n: Int): Int {
        if (n == 1) {
            return 5
        }
        var arr = intArrayOf(1, 1, 1, 1, 1)
        var sum = 5
        for (i in 2..n) {
            val copy = IntArray(5)
            for (j in arr.indices) {
                if (j == 0) {
                    copy[j] = sum
                } else {
                    copy[j] = copy[j - 1] - arr[j - 1]
                }
            }
            arr = copy.copyOf(5)
            sum = 0
            for (k in arr) {
                sum += k
            }
        }
        return sum
    }
}
```