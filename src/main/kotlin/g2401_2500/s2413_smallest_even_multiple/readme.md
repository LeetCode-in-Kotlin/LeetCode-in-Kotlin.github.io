[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2413\. Smallest Even Multiple

Easy

Given a **positive** integer `n`, return _the smallest positive integer that is a multiple of **both**_ `2` _and_ `n`.

**Example 1:**

**Input:** n = 5

**Output:** 10

**Explanation:** The smallest multiple of both 5 and 2 is 10. 

**Example 2:**

**Input:** n = 6

**Output:** 6

**Explanation:** The smallest multiple of both 6 and 2 is 6. Note that a number is a multiple of itself. 

**Constraints:**

*   `1 <= n <= 150`

## Solution

```kotlin
class Solution {
    fun smallestEvenMultiple(n: Int): Int {
        return if (n % 2 == 0) n else n * 2
    }
}
```