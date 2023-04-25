[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 927\. Three Equal Parts

Hard

You are given an array `arr` which consists of only zeros and ones, divide the array into **three non-empty parts** such that all of these parts represent the same binary value.

If it is possible, return any `[i, j]` with `i + 1 < j`, such that:

*   `arr[0], arr[1], ..., arr[i]` is the first part,
*   `arr[i + 1], arr[i + 2], ..., arr[j - 1]` is the second part, and
*   `arr[j], arr[j + 1], ..., arr[arr.length - 1]` is the third part.
*   All three parts have equal binary values.

If it is not possible, return `[-1, -1]`.

Note that the entire part is used when considering what binary value it represents. For example, `[1,1,0]` represents `6` in decimal, not `3`. Also, leading zeros **are allowed**, so `[0,1,1]` and `[1,1]` represent the same value.

**Example 1:**

**Input:** arr = [1,0,1,0,1]

**Output:** [0,3]

**Example 2:**

**Input:** arr = [1,1,0,1,1]

**Output:** [-1,-1]

**Example 3:**

**Input:** arr = [1,1,0,0,1]

**Output:** [0,2]

**Constraints:**

*   <code>3 <= arr.length <= 3 * 10<sup>4</sup></code>
*   `arr[i]` is `0` or `1`

## Solution

```kotlin
class Solution {
    fun threeEqualParts(arr: IntArray): IntArray {
        var ones = 0
        for (num in arr) {
            ones += num
        }
        if (ones == 0) {
            return intArrayOf(0, 2)
        } else if (ones % 3 != 0) {
            return intArrayOf(-1, -1)
        }
        ones /= 3
        var index1 = -1
        var index2 = -1
        var index3 = -1
        var totalOnes = 0
        for (i in arr.indices) {
            if (arr[i] == 0) {
                continue
            }
            totalOnes += arr[i]
            when (totalOnes) {
                1 -> index1 = i
                ones + 1 -> index2 = i
                2 * ones + 1 -> index3 = i
            }
        }
        while (index3 < arr.size) {
            if (arr[index1] == arr[index3] && arr[index2] == arr[index3]) {
                ++index1
                ++index2
                ++index3
            } else {
                return intArrayOf(-1, -1)
            }
        }
        return intArrayOf(index1 - 1, index2)
    }
}
```