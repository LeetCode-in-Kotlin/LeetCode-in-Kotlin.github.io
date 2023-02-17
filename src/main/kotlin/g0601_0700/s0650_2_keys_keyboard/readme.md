[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 650\. 2 Keys Keyboard

Medium

There is only one character `'A'` on the screen of a notepad. You can perform one of two operations on this notepad for each step:

*   Copy All: You can copy all the characters present on the screen (a partial copy is not allowed).
*   Paste: You can paste the characters which are copied last time.

Given an integer `n`, return _the minimum number of operations to get the character_ `'A'` _exactly_ `n` _times on the screen_.

**Example 1:**

**Input:** n = 3

**Output:** 3

**Explanation:** Initially, we have one character 'A'. 

In step 1, we use Copy All operation. 

In step 2, we use Paste operation to get 'AA'. 

In step 3, we use Paste operation to get 'AAA'.

**Example 2:**

**Input:** n = 1

**Output:** 0

**Constraints:**

*   `1 <= n <= 1000`

## Solution

```kotlin
class Solution {
    fun minSteps(n: Int): Int {
        var count = 1
        var cost = 0
        var addValue = 1
        while (count < n) {
            cost++
            count += addValue
            if (n % count == 0) {
                cost++
                addValue = count
            }
        }
        return cost
    }
}
```