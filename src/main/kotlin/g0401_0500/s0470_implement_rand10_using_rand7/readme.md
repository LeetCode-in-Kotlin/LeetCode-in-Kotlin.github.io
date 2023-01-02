[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 470\. Implement Rand10() Using Rand7()

Medium

Given the **API** `rand7()` that generates a uniform random integer in the range `[1, 7]`, write a function `rand10()` that generates a uniform random integer in the range `[1, 10]`. You can only call the API `rand7()`, and you shouldn't call any other API. Please **do not** use a language's built-in random API.

Each test case will have one **internal** argument `n`, the number of times that your implemented function `rand10()` will be called while testing. Note that this is **not an argument** passed to `rand10()`.

**Example 1:**

**Input:** n = 1

**Output:** [2]

**Example 2:**

**Input:** n = 2

**Output:** [2,8]

**Example 3:**

**Input:** n = 3

**Output:** [3,8,10]

**Constraints:**

*   <code>1 <= n <= 10<sup>5</sup></code>

**Follow up:**

*   What is the [expected value](https://en.wikipedia.org/wiki/Expected_value) for the number of calls to `rand7()` function?
*   Could you minimize the number of calls to `rand7()`?

## Solution

```kotlin
import java.util.Random

/*
 * The rand7() API is already defined in the parent class SolBase.
 * fun rand7(): Int {}
 * @return a random integer in the range 1 to 7
 */
@Suppress("kotlin:S2245")
class Solution {
    private val random: Random = Random()
    fun rand10(): Int {
        var r1: Int
        do {
            var r2: Int
            do { r2 = rand7() } while (r2 == 7)
            if (r2 in 1..3) { r1 = rand7() } else { r1 = 7 + rand7() }
        } while (r1 > 10)
        return r1
    }

    private fun rand7(): Int {
        return random.nextInt(7) + 1
    }
}
```