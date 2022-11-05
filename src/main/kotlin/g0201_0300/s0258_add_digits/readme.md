[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 258\. Add Digits

Easy

Given an integer `num`, repeatedly add all its digits until the result has only one digit, and return it.

**Example 1:**

**Input:** num = 38

**Output:** 2

**Explanation:** The process is 38 --> 3 + 8 --> 11 11 --> 1 + 1 --> 2 Since 2 has only one digit, return it.

**Example 2:**

**Input:** num = 0

**Output:** 0

**Constraints:**

*   <code>0 <= num <= 2<sup>31</sup> - 1</code>

**Follow up:** Could you do it without any loop/recursion in `O(1)` runtime?

## Solution

```kotlin
class Solution {
    fun addDigits(num: Int): Int {
        if (num == 0) {
            return 0
        }
        return if (num % 9 == 0) {
            9
        } else num % 9
    }
}
```