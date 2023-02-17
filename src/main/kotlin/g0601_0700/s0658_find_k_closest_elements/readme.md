[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 658\. Find K Closest Elements

Medium

Given a **sorted** integer array `arr`, two integers `k` and `x`, return the `k` closest integers to `x` in the array. The result should also be sorted in ascending order.

An integer `a` is closer to `x` than an integer `b` if:

*   `|a - x| < |b - x|`, or
*   `|a - x| == |b - x|` and `a < b`

**Example 1:**

**Input:** arr = [1,2,3,4,5], k = 4, x = 3

**Output:** [1,2,3,4]

**Example 2:**

**Input:** arr = [1,2,3,4,5], k = 4, x = -1

**Output:** [1,2,3,4]

**Constraints:**

*   `1 <= k <= arr.length`
*   <code>1 <= arr.length <= 10<sup>4</sup></code>
*   `arr` is sorted in **ascending** order.
*   <code>-10<sup>4</sup> <= arr[i], x <= 10<sup>4</sup></code>

## Solution

```kotlin
class Solution {
    fun findClosestElements(arr: IntArray, k: Int, x: Int): List<Int> {
        var left = 0
        var right = arr.size - k
        val answer: MutableList<Int> = ArrayList()
        while (left < right) {
            val mid = left + (right - left) / 2
            if (x - arr[mid] > arr[mid + k] - x) {
                left = mid + 1
            } else {
                right = mid
            }
        }
        for (i in left until left + k) {
            answer.add(arr[i])
        }
        return answer
    }
}
```