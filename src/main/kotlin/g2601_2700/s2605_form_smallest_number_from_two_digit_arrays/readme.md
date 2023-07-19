[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2605\. Form Smallest Number From Two Digit Arrays

Easy

Given two arrays of **unique** digits `nums1` and `nums2`, return _the **smallest** number that contains **at least** one digit from each array_.

**Example 1:**

**Input:** nums1 = [4,1,3], nums2 = [5,7]

**Output:** 15

**Explanation:** The number 15 contains the digit 1 from nums1 and the digit 5 from nums2. It can be proven that 15 is the smallest number we can have.

**Example 2:**

**Input:** nums1 = [3,5,2,6], nums2 = [3,1,7]

**Output:** 3

**Explanation:** The number 3 contains the digit 3 which exists in both arrays.

**Constraints:**

*   `1 <= nums1.length, nums2.length <= 9`
*   `1 <= nums1[i], nums2[i] <= 9`
*   All digits in each array are **unique**.

## Solution

```kotlin
class Solution {
    fun minNumber(nums1: IntArray, nums2: IntArray): Int {
        val set = HashSet<Int>()
        var (min, min1, min2) = arrayOf(10, 10, 10)
        for (num in nums1) {
            min1 = minOf(min1, num)
            set.add(num)
        }
        for (num in nums2) {
            min2 = minOf(min2, num)
            if (set.contains(num)) min = minOf(min, num)
        }
        if (min != 10) return min
        return minOf(min1, min2) * 10 + maxOf(min1, min2)
    }
}
```