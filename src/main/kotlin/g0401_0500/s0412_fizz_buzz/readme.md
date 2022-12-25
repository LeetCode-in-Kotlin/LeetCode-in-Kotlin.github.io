[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 412\. Fizz Buzz

Easy

Given an integer `n`, return _a string array_ `answer` _(**1-indexed**) where_:

*   `answer[i] == "FizzBuzz"` if `i` is divisible by `3` and `5`.
*   `answer[i] == "Fizz"` if `i` is divisible by `3`.
*   `answer[i] == "Buzz"` if `i` is divisible by `5`.
*   `answer[i] == i` (as a string) if none of the above conditions are true.

**Example 1:**

**Input:** n = 3

**Output:** ["1","2","Fizz"]

**Example 2:**

**Input:** n = 5

**Output:** ["1","2","Fizz","4","Buzz"]

**Example 3:**

**Input:** n = 15

**Output:** ["1","2","Fizz","4","Buzz","Fizz","7","8","Fizz","Buzz","11","Fizz","13","14","FizzBuzz"]

**Constraints:**

*   <code>1 <= n <= 10<sup>4</sup></code>

## Solution

```kotlin
class Solution {
    fun fizzBuzz(n: Int): Array<String> = Array<String>(n) { index ->
        val value = index + 1
        when {
            (value % 3 == 0 && value % 5 == 0) -> "FizzBuzz"
            (value % 3 == 0) -> "Fizz"
            (value % 5 == 0) -> "Buzz"
            else -> "$value"
        }
    }
}
```