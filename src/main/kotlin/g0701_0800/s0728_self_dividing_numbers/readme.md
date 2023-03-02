[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 728\. Self Dividing Numbers

Easy

A **self-dividing number** is a number that is divisible by every digit it contains.

*   For example, `128` is **a self-dividing number** because `128 % 1 == 0`, `128 % 2 == 0`, and `128 % 8 == 0`.

A **self-dividing number** is not allowed to contain the digit zero.

Given two integers `left` and `right`, return _a list of all the **self-dividing numbers** in the range_ `[left, right]`.

**Example 1:**

**Input:** left = 1, right = 22

**Output:** [1,2,3,4,5,6,7,8,9,11,12,15,22]

**Example 2:**

**Input:** left = 47, right = 85

**Output:** [48,55,66,77]

**Constraints:**

*   <code>1 <= left <= right <= 10<sup>4</sup></code>

## Solution

```kotlin
@Suppress("NAME_SHADOWING")
class Solution {
    fun selfDividingNumbers(left: Int, right: Int): List<Int> {
        val list: MutableList<Int> = ArrayList()
        for (i in left..right) {
            if (isSelfDividing(i)) {
                list.add(i)
            }
        }
        return list
    }

    private fun isSelfDividing(value: Int): Boolean {
        var value = value
        val origin = value
        while (value != 0) {
            val digit = value % 10
            value /= 10
            if (digit == 0 || origin % digit != 0) {
                return false
            }
        }
        return true
    }
}
```