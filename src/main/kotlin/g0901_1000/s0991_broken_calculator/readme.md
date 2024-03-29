[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 991\. Broken Calculator

Medium

There is a broken calculator that has the integer `startValue` on its display initially. In one operation, you can:

*   multiply the number on display by `2`, or
*   subtract `1` from the number on display.

Given two integers `startValue` and `target`, return _the minimum number of operations needed to display_ `target` _on the calculator_.

**Example 1:**

**Input:** startValue = 2, target = 3

**Output:** 2

**Explanation:** Use double operation and then decrement operation {2 -> 4 -> 3}.

**Example 2:**

**Input:** startValue = 5, target = 8

**Output:** 2

**Explanation:** Use decrement and then double {5 -> 4 -> 8}.

**Example 3:**

**Input:** startValue = 3, target = 10

**Output:** 3

**Explanation:** Use double, decrement and double {3 -> 6 -> 5 -> 10}.

**Constraints:**

*   <code>1 <= startValue, target <= 10<sup>9</sup></code>

## Solution

```kotlin
@Suppress("NAME_SHADOWING")
class Solution {
    fun brokenCalc(startValue: Int, target: Int): Int {
        var target = target
        var result = 0
        while (startValue != target) {
            if (target > startValue && target % 2 != 0) {
                target += 1
                result++
            } else if (target > startValue) {
                target /= 2
                result++
            } else {
                result += startValue - target
                break
            }
        }
        return result
    }
}
```