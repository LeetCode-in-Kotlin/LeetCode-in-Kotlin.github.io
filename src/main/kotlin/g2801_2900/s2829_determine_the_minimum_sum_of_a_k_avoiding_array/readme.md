[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2829\. Determine the Minimum Sum of a k-avoiding Array

Medium

You are given two integers, `n` and `k`.

An array of **distinct** positive integers is called a **k-avoiding** array if there does not exist any pair of distinct elements that sum to `k`.

Return _the **minimum** possible sum of a k-avoiding array of length_ `n`.

**Example 1:**

**Input:** n = 5, k = 4

**Output:** 18

**Explanation:** Consider the k-avoiding array [1,2,4,5,6], which has a sum of 18. It can be proven that there is no k-avoiding array with a sum less than 18.

**Example 2:**

**Input:** n = 2, k = 6

**Output:** 3

**Explanation:** We can construct the array [1,2], which has a sum of 3. It can be proven that there is no k-avoiding array with a sum less than 3.

**Constraints:**

*   `1 <= n, k <= 50`

## Solution

```kotlin
@Suppress("NAME_SHADOWING")
class Solution {
    fun minimumSum(n: Int, k: Int): Int {
        var k = k
        val arr = IntArray(n)
        val a = k / 2
        var sum = 0
        if (a > n) {
            for (i in 0 until n) {
                arr[i] = i + 1
                sum += arr[i]
            }
        } else {
            for (i in 0 until a) {
                arr[i] = i + 1
                sum += arr[i]
            }
            for (j in a until n) {
                arr[j] = k++
                sum += arr[j]
            }
        }
        return sum
    }
}
```