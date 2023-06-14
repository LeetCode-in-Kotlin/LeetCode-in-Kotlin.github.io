[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1534\. Count Good Triplets

Easy

Given an array of integers `arr`, and three integers `a`, `b` and `c`. You need to find the number of good triplets.

A triplet `(arr[i], arr[j], arr[k])` is **good** if the following conditions are true:

*   `0 <= i < j < k < arr.length`
*   `|arr[i] - arr[j]| <= a`
*   `|arr[j] - arr[k]| <= b`
*   `|arr[i] - arr[k]| <= c`

Where `|x|` denotes the absolute value of `x`.

Return _the number of good triplets_.

**Example 1:**

**Input:** arr = [3,0,1,1,9,7], a = 7, b = 2, c = 3

**Output:** 4

**Explanation:** There are 4 good triplets: [(3,0,1), (3,0,1), (3,1,1), (0,1,1)].

**Example 2:**

**Input:** arr = [1,1,2,2,3], a = 0, b = 0, c = 1

**Output:** 0

**Explanation:** No triplet satisfies all conditions.

**Constraints:**

*   `3 <= arr.length <= 100`
*   `0 <= arr[i] <= 1000`
*   `0 <= a, b, c <= 1000`

## Solution

```kotlin
class Solution {
    fun countGoodTriplets(arr: IntArray, a: Int, b: Int, c: Int): Int {
        var count = 0
        for (i in 0 until arr.size - 2) {
            for (j in i + 1 until arr.size - 1) {
                if (Math.abs(arr[i] - arr[j]) <= a) {
                    for (k in j + 1 until arr.size) {
                        if (Math.abs(arr[j] - arr[k]) <= b && Math.abs(arr[i] - arr[k]) <= c) {
                            count++
                        }
                    }
                }
            }
        }
        return count
    }
}
```