[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1537\. Get the Maximum Score

Hard

You are given two **sorted** arrays of distinct integers `nums1` and `nums2.`

A **valid path** is defined as follows:

*   Choose array `nums1` or `nums2` to traverse (from index-0).
*   Traverse the current array from left to right.
*   If you are reading any value that is present in `nums1` and `nums2` you are allowed to change your path to the other array. (Only one repeated value is considered in the valid path).

The **score** is defined as the sum of uniques values in a valid path.

Return _the maximum score you can obtain of all possible **valid paths**_. Since the answer may be too large, return it modulo <code>10<sup>9</sup> + 7</code>.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/07/16/sample_1_1893.png)

**Input:** nums1 = [2,4,5,8,10], nums2 = [4,6,8,9]

**Output:** 30

**Explanation:** Valid paths: 

[2,4,5,8,10], [2,4,5,8,9], [2,4,6,8,9], [2,4,6,8,10], (starting from nums1) 

[4,6,8,9], [4,5,8,10], [4,5,8,9], [4,6,8,10] (starting from nums2) 

The maximum is obtained with the path in green **[2,4,6,8,10]**.

**Example 2:**

**Input:** nums1 = [1,3,5,7,9], nums2 = [3,5,100]

**Output:** 109

**Explanation:** Maximum sum is obtained with the path **[1,3,5,100]**.

**Example 3:**

**Input:** nums1 = [1,2,3,4,5], nums2 = [6,7,8,9,10]

**Output:** 40

**Explanation:** There are no common elements between nums1 and nums2. Maximum sum is obtained with the path [6,7,8,9,10].

**Constraints:**

*   <code>1 <= nums1.length, nums2.length <= 10<sup>5</sup></code>
*   <code>1 <= nums1[i], nums2[i] <= 10<sup>7</sup></code>
*   `nums1` and `nums2` are strictly increasing.

## Solution

```kotlin
class Solution {
    fun maxSum(nums1: IntArray, nums2: IntArray): Int {
        val mod = 1000000007
        var result: Long = 0
        var start1 = 0
        var start2 = 0
        var sum1: Long = 0
        var sum2: Long = 0
        while (start1 < nums1.size && start2 < nums2.size) {
            if (nums1[start1] < nums2[start2]) {
                sum1 += nums1[start1].toLong()
                start1++
            } else if (nums1[start1] > nums2[start2]) {
                sum2 += nums2[start2].toLong()
                start2++
            } else {
                result += Math.max(sum1, sum2) + nums1[start1]
                start1++
                start2++
                sum1 = 0
                sum2 = 0
            }
        }
        while (start1 < nums1.size) {
            sum1 += nums1[start1].toLong()
            start1++
        }
        while (start2 < nums2.size) {
            sum2 += nums2[start2].toLong()
            start2++
        }
        return ((Math.max(sum1, sum2) + result) % mod).toInt()
    }
}
```