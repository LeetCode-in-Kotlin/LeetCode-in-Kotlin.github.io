[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 941\. Valid Mountain Array

Easy

Given an array of integers `arr`, return _`true` if and only if it is a valid mountain array_.

Recall that arr is a mountain array if and only if:

*   `arr.length >= 3`
*   There exists some `i` with `0 < i < arr.length - 1` such that:
    *   `arr[0] < arr[1] < ... < arr[i - 1] < arr[i]`
    *   `arr[i] > arr[i + 1] > ... > arr[arr.length - 1]`

![](https://assets.leetcode.com/uploads/2019/10/20/hint_valid_mountain_array.png)

**Example 1:**

**Input:** arr = [2,1]

**Output:** false

**Example 2:**

**Input:** arr = [3,5,5]

**Output:** false

**Example 3:**

**Input:** arr = [0,3,2,1]

**Output:** true

**Constraints:**

*   <code>1 <= arr.length <= 10<sup>4</sup></code>
*   <code>0 <= arr[i] <= 10<sup>4</sup></code>

## Solution

```kotlin
class Solution {
    fun validMountainArray(arr: IntArray): Boolean {
        var i = 0
        var flag1 = false
        var flag2 = false
        while (i < arr.size - 1 && arr[i] < arr[i + 1]) {
            flag1 = true
            i++
        }
        while (i < arr.size - 1 && arr[i] > arr[i + 1]) {
            flag2 = true
            i++
        }
        if (i < arr.size - 1) {
            return false
        }
        return !(!flag1 || !flag2)
    }
}
```