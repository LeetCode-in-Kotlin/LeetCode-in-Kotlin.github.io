[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 386\. Lexicographical Numbers

Medium

Given an integer `n`, return all the numbers in the range `[1, n]` sorted in lexicographical order.

You must write an algorithm that runs in `O(n)` time and uses `O(1)` extra space.

**Example 1:**

**Input:** n = 13

**Output:** [1,10,11,12,13,2,3,4,5,6,7,8,9]

**Example 2:**

**Input:** n = 2

**Output:** [1,2]

**Constraints:**

*   <code>1 <= n <= 5 * 10<sup>4</sup></code>

## Solution

```kotlin
class Solution {
    fun lexicalOrder(n: Int): List<Int> {
        val list = ArrayList<Int>()
        fun recursion(x: Int) {
            if (x > n) return
            list.add(x)
            for (i in 0..9) {
                recursion(x * 10 + i)
            }
        }
        for (i in 1..9) {
            recursion(i)
        }
        return list
    }
}
```