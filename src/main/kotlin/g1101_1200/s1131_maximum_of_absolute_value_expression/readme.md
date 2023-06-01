[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1131\. Maximum of Absolute Value Expression

Medium

Given two arrays of integers with equal lengths, return the maximum value of:

`|arr1[i] - arr1[j]| + |arr2[i] - arr2[j]| + |i - j|`

where the maximum is taken over all `0 <= i, j < arr1.length`.

**Example 1:**

**Input:** arr1 = [1,2,3,4], arr2 = [-1,4,5,6]

**Output:** 13

**Example 2:**

**Input:** arr1 = [1,-2,-5,0,10], arr2 = [0,-2,-1,-7,-4]

**Output:** 20

**Constraints:**

*   `2 <= arr1.length == arr2.length <= 40000`
*   `-10^6 <= arr1[i], arr2[i] <= 10^6`

## Solution

```kotlin
class Solution {
    fun maxAbsValExpr(arr1: IntArray, arr2: IntArray): Int {
        if (arr1.size != arr2.size) {
            return 0
        }
        var max1 = Int.MIN_VALUE
        var max2 = Int.MIN_VALUE
        var max3 = Int.MIN_VALUE
        var max4 = Int.MIN_VALUE
        var min1 = Int.MAX_VALUE
        var min2 = Int.MAX_VALUE
        var min3 = Int.MAX_VALUE
        var min4 = Int.MAX_VALUE
        for (i in arr1.indices) {
            max1 = Math.max(arr1[i] + arr2[i] + i, max1)
            min1 = Math.min(arr1[i] + arr2[i] + i, min1)
            max2 = Math.max(i - arr1[i] - arr2[i], max2)
            min2 = Math.min(i - arr1[i] - arr2[i], min2)
            max3 = Math.max(arr1[i] - arr2[i] + i, max3)
            min3 = Math.min(arr1[i] - arr2[i] + i, min3)
            max4 = Math.max(arr2[i] - arr1[i] + i, max4)
            min4 = Math.min(arr2[i] - arr1[i] + i, min4)
        }
        return Math.max(Math.max(max1 - min1, max2 - min2), Math.max(max3 - min3, max4 - min4))
    }
}
```