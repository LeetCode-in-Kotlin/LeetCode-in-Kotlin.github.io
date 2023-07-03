[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2081\. Sum of k-Mirror Numbers

Hard

A **k-mirror number** is a **positive** integer **without leading zeros** that reads the same both forward and backward in base-10 **as well as** in base-k.

*   For example, `9` is a 2-mirror number. The representation of `9` in base-10 and base-2 are `9` and `1001` respectively, which read the same both forward and backward.
*   On the contrary, `4` is not a 2-mirror number. The representation of `4` in base-2 is `100`, which does not read the same both forward and backward.

Given the base `k` and the number `n`, return _the **sum** of the_ `n` _**smallest** k-mirror numbers_.

**Example 1:**

**Input:** k = 2, n = 5

**Output:** 25

**Explanation:**

The 5 smallest 2-mirror numbers and their representations in base-2 are listed as follows:

    base-10 base-2
        1    1
        3    11
        5    101
        7    111
        9    1001
    Their sum = 1 + 3 + 5 + 7 + 9 = 25. 

**Example 2:**

**Input:** k = 3, n = 7

**Output:** 499

**Explanation:** The 7 smallest 3-mirror numbers are and their representations in base-3 are listed as follows:

    base-10 base-3
        1    1
        2    2
        4    11
        8    22
        121  11111
        151  12121
        212  21212
    Their sum = 1 + 2 + 4 + 8 + 121 + 151 + 212 = 499. 

**Example 3:**

**Input:** k = 7, n = 17

**Output:** 20379000

**Explanation:** The 17 smallest 7-mirror numbers are:

1, 2, 3, 4, 5, 6, 8, 121, 171, 242, 292, 16561, 65656, 2137312, 4602064, 6597956, 6958596 

**Constraints:**

*   `2 <= k <= 9`
*   `1 <= n <= 30`

## Solution

```kotlin
class Solution {
    fun kMirror(k: Int, n: Int): Long {
        val result: MutableList<Long> = ArrayList()
        var len = 1
        while (result.size < n) {
            backtrack(result, CharArray(len++), k, n, 0)
        }
        var sum: Long = 0
        for (num in result) {
            sum += num
        }
        return sum
    }

    private fun backtrack(result: MutableList<Long>, arr: CharArray, k: Int, n: Int, index: Int) {
        if (result.size == n) {
            return
        }
        if (index >= (arr.size + 1) / 2) {
            // Number in base-10
            val number = String(arr).toLong(k)
            if (isPalindrome(number)) {
                result.add(number)
            }
            return
        }
        // Generate base-k palindrome number in arr.length without leading zeros
        for (i in 0 until k) {
            if (index == 0 && i == 0) {
                // Leading zeros
                continue
            }
            val c: Char = (i + '0'.code).toChar()
            arr[index] = c
            arr[arr.size - 1 - index] = c
            backtrack(result, arr, k, n, index + 1)
        }
    }

    private fun isPalindrome(number: Long): Boolean {
        val strNum = number.toString()
        var left = 0
        var right = strNum.length - 1
        while (left < right) {
            if (strNum[left] == strNum[right]) {
                left++
                right--
            } else {
                return false
            }
        }
        return true
    }
}
```