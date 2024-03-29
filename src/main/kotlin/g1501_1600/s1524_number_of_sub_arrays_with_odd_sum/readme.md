[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1524\. Number of Sub-arrays With Odd Sum

Medium

Given an array of integers `arr`, return _the number of subarrays with an **odd** sum_.

Since the answer can be very large, return it modulo <code>10<sup>9</sup> + 7</code>.

**Example 1:**

**Input:** arr = [1,3,5]

**Output:** 4

**Explanation:** All subarrays are [[1],[1,3],[1,3,5],[3],[3,5],[5]] 

All sub-arrays sum are [1,4,9,3,8,5]. 

Odd sums are [1,9,3,5] so the answer is 4.

**Example 2:**

**Input:** arr = [2,4,6]

**Output:** 0

**Explanation:** All subarrays are [[2],[2,4],[2,4,6],[4],[4,6],[6]] 

All sub-arrays sum are [2,6,12,4,10,6]. 

All sub-arrays have even sum and the answer is 0.

**Example 3:**

**Input:** arr = [1,2,3,4,5,6,7]

**Output:** 16

**Constraints:**

*   <code>1 <= arr.length <= 10<sup>5</sup></code>
*   `1 <= arr[i] <= 100`

## Solution

```kotlin
class Solution {
    fun numOfSubarrays(arr: IntArray): Int {
        var number = if (arr[0] % 2 == 0) 0 else 1
        var res = number.toLong()
        for (i in 1 until arr.size) {
            if (arr[i] % 2 != 0) {
                number = i - number + 1
            }
            res += number.toLong()
        }
        val mod: Long = 1000000007
        return (res % mod).toInt()
    }
}
```