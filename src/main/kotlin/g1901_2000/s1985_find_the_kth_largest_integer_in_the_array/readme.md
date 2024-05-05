[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1985\. Find the Kth Largest Integer in the Array

Medium

You are given an array of strings `nums` and an integer `k`. Each string in `nums` represents an integer without leading zeros.

Return _the string that represents the_ <code>k<sup>th</sup></code> _**largest integer** in_ `nums`.

**Note**: Duplicate numbers should be counted distinctly. For example, if `nums` is `["1","2","2"]`, `"2"` is the first largest integer, `"2"` is the second-largest integer, and `"1"` is the third-largest integer.

**Example 1:**

**Input:** nums = ["3","6","7","10"], k = 4

**Output:** "3"

**Explanation:** 

The numbers in nums sorted in non-decreasing order are ["3","6","7","10"]. 

The 4<sup>th</sup> largest integer in nums is "3".

**Example 2:**

**Input:** nums = ["2","21","12","1"], k = 3

**Output:** "2"

**Explanation:** 

The numbers in nums sorted in non-decreasing order are ["1","2","12","21"]. 

The 3<sup>rd</sup> largest integer in nums is "2".

**Example 3:**

**Input:** nums = ["0","0"], k = 2

**Output:** "0"

**Explanation:** 

The numbers in nums sorted in non-decreasing order are ["0","0"]. 

The 2<sup>nd</sup> largest integer in nums is "0".

**Constraints:**

*   <code>1 <= k <= nums.length <= 10<sup>4</sup></code>
*   `1 <= nums[i].length <= 100`
*   `nums[i]` consists of only digits.
*   `nums[i]` will not have any leading zeros.

## Solution

```kotlin
class Solution {
    fun kthLargestNumber(nums: Array<String>, k: Int): String {
        nums.sortWith { n1: String, n2: String -> compareStringInt(n2, n1) }
        return nums[k - 1]
    }

    private fun compareStringInt(n1: String, n2: String): Int {
        if (n1.length != n2.length) {
            return if (n1.length < n2.length) -1 else 1
        }
        for (i in n1.indices) {
            val n1Digit = n1[i].code - '0'.code
            val n2Digit = n2[i].code - '0'.code
            if (n1Digit > n2Digit) {
                return 1
            } else if (n2Digit > n1Digit) {
                return -1
            }
        }
        return 0
    }
}
```