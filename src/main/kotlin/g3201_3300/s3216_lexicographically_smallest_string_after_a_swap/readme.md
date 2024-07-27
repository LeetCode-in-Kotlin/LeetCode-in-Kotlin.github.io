[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3216\. Lexicographically Smallest String After a Swap

Easy

Given a string `s` containing only digits, return the lexicographically smallest string that can be obtained after swapping **adjacent** digits in `s` with the same **parity** at most **once**.

Digits have the same parity if both are odd or both are even. For example, 5 and 9, as well as 2 and 4, have the same parity, while 6 and 9 do not.

**Example 1:**

**Input:** s = "45320"

**Output:** "43520"

**Explanation:**

`s[1] == '5'` and `s[2] == '3'` both have the same parity, and swapping them results in the lexicographically smallest string.

**Example 2:**

**Input:** s = "001"

**Output:** "001"

**Explanation:**

There is no need to perform a swap because `s` is already the lexicographically smallest.

**Constraints:**

*   `2 <= s.length <= 100`
*   `s` consists only of digits.

## Solution

```kotlin
class Solution {
    fun getSmallestString(s: String): String {
        val arr = s.toCharArray()
        for (i in 1 until arr.size) {
            if (arr[i - 1].code % 2 == arr[i].code % 2 && arr[i - 1] > arr[i]) {
                val temp = arr[i]
                arr[i] = arr[i - 1]
                arr[i - 1] = temp
                break
            }
        }
        return String(arr)
    }
}
```