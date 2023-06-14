[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1287\. Element Appearing More Than 25% In Sorted Array

Easy

Given an integer array **sorted** in non-decreasing order, there is exactly one integer in the array that occurs more than 25% of the time, return that integer.

**Example 1:**

**Input:** arr = [1,2,2,6,6,6,6,7,10]

**Output:** 6

**Example 2:**

**Input:** arr = [1,1]

**Output:** 1

**Constraints:**

*   <code>1 <= arr.length <= 10<sup>4</sup></code>
*   <code>0 <= arr[i] <= 10<sup>5</sup></code>

## Solution

```kotlin
class Solution {
    fun findSpecialInteger(arr: IntArray): Int {
        val quarter = arr.size / 4
        for (i in 0 until arr.size - quarter) {
            if (arr[i] == arr[i + quarter]) {
                return arr[i]
            }
        }
        return -1
    }
}
```