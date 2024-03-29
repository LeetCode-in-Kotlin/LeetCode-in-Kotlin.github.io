[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2274\. Maximum Consecutive Floors Without Special Floors

Medium

Alice manages a company and has rented some floors of a building as office space. Alice has decided some of these floors should be **special floors**, used for relaxation only.

You are given two integers `bottom` and `top`, which denote that Alice has rented all the floors from `bottom` to `top` (**inclusive**). You are also given the integer array `special`, where `special[i]` denotes a special floor that Alice has designated for relaxation.

Return _the **maximum** number of consecutive floors without a special floor_.

**Example 1:**

**Input:** bottom = 2, top = 9, special = [4,6]

**Output:** 3

**Explanation:** The following are the ranges (inclusive) of consecutive floors without a special floor:

- (2, 3) with a total amount of 2 floors.

- (5, 5) with a total amount of 1 floor.

- (7, 9) with a total amount of 3 floors.

Therefore, we return the maximum number which is 3 floors. 

**Example 2:**

**Input:** bottom = 6, top = 8, special = [7,6,8]

**Output:** 0

**Explanation:** Every floor rented is a special floor, so we return 0. 

**Constraints:**

*   <code>1 <= special.length <= 10<sup>5</sup></code>
*   <code>1 <= bottom <= special[i] <= top <= 10<sup>9</sup></code>
*   All the values of `special` are **unique**.

## Solution

```kotlin
class Solution {
    fun maxConsecutive(bottom: Int, top: Int, special: IntArray): Int {
        special.sort()
        var start = bottom
        var ans = 0
        for (j in special) {
            if (j - start > ans) {
                ans = j - start
                start = j + 1
            } else {
                start = j + 1
            }
        }
        if (ans < top - special[special.size - 1]) {
            ans = top - special[special.size - 1]
        }
        return ans
    }
}
```