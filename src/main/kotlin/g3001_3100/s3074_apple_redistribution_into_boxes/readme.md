[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3074\. Apple Redistribution into Boxes

Easy

You are given an array `apple` of size `n` and an array `capacity` of size `m`.

There are `n` packs where the <code>i<sup>th</sup></code> pack contains `apple[i]` apples. There are `m` boxes as well, and the <code>i<sup>th</sup></code> box has a capacity of `capacity[i]` apples.

Return _the **minimum** number of boxes you need to select to redistribute these_ `n` _packs of apples into boxes_.

**Note** that, apples from the same pack can be distributed into different boxes.

**Example 1:**

**Input:** apple = [1,3,2], capacity = [4,3,1,5,2]

**Output:** 2

**Explanation:** We will use boxes with capacities 4 and 5. It is possible to distribute the apples as the total capacity is greater than or equal to the total number of apples.

**Example 2:**

**Input:** apple = [5,5,5], capacity = [2,4,2,7]

**Output:** 4

**Explanation:** We will need to use all the boxes.

**Constraints:**

*   `1 <= n == apple.length <= 50`
*   `1 <= m == capacity.length <= 50`
*   `1 <= apple[i], capacity[i] <= 50`
*   The input is generated such that it's possible to redistribute packs of apples into boxes.

## Solution

```kotlin
import kotlin.math.max

class Solution {
    fun minimumBoxes(apple: IntArray, capacity: IntArray): Int {
        val count = IntArray(51)
        var appleSum = 0
        for (j in apple) {
            appleSum += j
        }
        var reqCapacity = 0
        var max = 0
        for (j in capacity) {
            count[j]++
            max = max(max.toDouble(), j.toDouble()).toInt()
        }
        for (i in max downTo 0) {
            if (count[i] >= 1) {
                while (count[i] != 0) {
                    appleSum -= i
                    reqCapacity++
                    if (appleSum <= 0) {
                        return reqCapacity
                    }
                    count[i]--
                }
            }
        }
        return reqCapacity
    }
}
```