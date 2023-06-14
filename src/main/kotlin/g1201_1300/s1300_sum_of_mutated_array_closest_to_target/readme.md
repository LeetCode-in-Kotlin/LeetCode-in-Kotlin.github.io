[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1300\. Sum of Mutated Array Closest to Target

Medium

Given an integer array `arr` and a target value `target`, return the integer `value` such that when we change all the integers larger than `value` in the given array to be equal to `value`, the sum of the array gets as close as possible (in absolute difference) to `target`.

In case of a tie, return the minimum such integer.

Notice that the answer is not neccesarilly a number from `arr`.

**Example 1:**

**Input:** arr = [4,9,3], target = 10

**Output:** 3

**Explanation:** When using 3 arr converts to [3, 3, 3] which sums 9 and that's the optimal answer.

**Example 2:**

**Input:** arr = [2,3,5], target = 10

**Output:** 5

**Example 3:**

**Input:** arr = [60864,25176,27249,21296,20204], target = 56803

**Output:** 11361

**Constraints:**

*   <code>1 <= arr.length <= 10<sup>4</sup></code>
*   <code>1 <= arr[i], target <= 10<sup>5</sup></code>

## Solution

```kotlin
class Solution {
    fun findBestValue(arr: IntArray, target: Int): Int {
        arr.sort()
        val n = arr.size
        var lo = 0
        var hi = arr[n - 1]
        var min = Int.MAX_VALUE
        var ans = -1
        while (lo <= hi) {
            val mid = (lo + hi) / 2
            val m = check(mid, arr, target)
            val l = check(mid - 1, arr, target)
            val r = check(mid + 1, arr, target)
            if (m < min || m == min && ans > mid) {
                min = m
                ans = mid
            } else if (l <= r) {
                hi = mid - 1
            } else {
                lo = mid + 1
            }
        }
        return ans
    }

    fun check(v: Int, arr: IntArray, target: Int): Int {
        var sum = 0
        for (i in arr.indices) {
            sum += if (arr[i] >= v) {
                return Math.abs(sum + (arr.size - i) * v - target)
            } else {
                arr[i]
            }
        }
        return Math.abs(sum - target)
    }
}
```