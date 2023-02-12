[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 605\. Can Place Flowers

Easy

You have a long flowerbed in which some of the plots are planted, and some are not. However, flowers cannot be planted in **adjacent** plots.

Given an integer array `flowerbed` containing `0`'s and `1`'s, where `0` means empty and `1` means not empty, and an integer `n`, return _if_ `n` new flowers can be planted in the `flowerbed` without violating the no-adjacent-flowers rule.

**Example 1:**

**Input:** flowerbed = [1,0,0,0,1], n = 1

**Output:** true

**Example 2:**

**Input:** flowerbed = [1,0,0,0,1], n = 2

**Output:** false

**Constraints:**

*   <code>1 <= flowerbed.length <= 2 * 10<sup>4</sup></code>
*   `flowerbed[i]` is `0` or `1`.
*   There are no two adjacent flowers in `flowerbed`.
*   `0 <= n <= flowerbed.length`

## Solution

```kotlin
@Suppress("NAME_SHADOWING")
class Solution {
    fun canPlaceFlowers(flowerbed: IntArray, n: Int): Boolean {
        var n = n
        for (i in flowerbed.indices) {
            if (flowerbed[i] == 0 && n > 0) {
                val left = i == 0 || flowerbed[i - 1] == 0
                val right = i == flowerbed.size - 1 || flowerbed[i + 1] == 0
                if (left && right) {
                    flowerbed[i] = 1
                    n--
                }
            }
            if (n == 0) {
                break
            }
        }
        return n == 0
    }
}
```