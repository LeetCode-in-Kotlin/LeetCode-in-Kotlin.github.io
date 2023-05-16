[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 982\. Triples with Bitwise AND Equal To Zero

Hard

Given an integer array nums, return _the number of **AND triples**_.

An **AND triple** is a triple of indices `(i, j, k)` such that:

*   `0 <= i < nums.length`
*   `0 <= j < nums.length`
*   `0 <= k < nums.length`
*   `nums[i] & nums[j] & nums[k] == 0`, where `&` represents the bitwise-AND operator.

**Example 1:**

**Input:** nums = [2,1,3]

**Output:** 12

**Explanation:** We could choose the following i, j, k triples:

(i=0, j=0, k=1) : 2 & 2 & 1

(i=0, j=1, k=0) : 2 & 1 & 2

(i=0, j=1, k=1) : 2 & 1 & 1

(i=0, j=1, k=2) : 2 & 1 & 3

(i=0, j=2, k=1) : 2 & 3 & 1

(i=1, j=0, k=0) : 1 & 2 & 2

(i=1, j=0, k=1) : 1 & 2 & 1

(i=1, j=0, k=2) : 1 & 2 & 3

(i=1, j=1, k=0) : 1 & 1 & 2

(i=1, j=2, k=0) : 1 & 3 & 2

(i=2, j=0, k=1) : 3 & 2 & 1

(i=2, j=1, k=0) : 3 & 1 & 2

**Example 2:**

**Input:** nums = [0,0,0]

**Output:** 27

**Constraints:**

*   `1 <= nums.length <= 1000`
*   <code>0 <= nums[i] < 2<sup>16</sup></code>

## Solution

```kotlin
class Solution {
    fun countTriplets(nums: IntArray): Int {
        val arr = IntArray(1 shl 17)
        for (num in nums) {
            var mask = 0
            for (i in 0..15) {
                if (num and (1 shl i) == 0) {
                    mask = mask or (1 shl i)
                }
            }
            var s = mask
            while (s > 0) {
                arr[s]++
                s = s - 1 and mask
            }
        }
        var count = 0
        for (j in nums) {
            for (num in nums) {
                val `val` = j and num
                if (`val` == 0) {
                    count += nums.size
                } else {
                    var mask = 0
                    for (k in 0..15) {
                        if (`val` and (1 shl k) > 0) {
                            mask = mask or (1 shl k)
                        }
                    }
                    count += arr[mask]
                }
            }
        }
        return count
    }
}
```