[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1577\. Number of Ways Where Square of Number Is Equal to Product of Two Numbers

Medium

Given two arrays of integers `nums1` and `nums2`, return the number of triplets formed (type 1 and type 2) under the following rules:

*   Type 1: Triplet (i, j, k) if <code>nums1[i]<sup>2</sup> == nums2[j] * nums2[k]</code> where `0 <= i < nums1.length` and `0 <= j < k < nums2.length`.
*   Type 2: Triplet (i, j, k) if <code>nums2[i]<sup>2</sup> == nums1[j] * nums1[k]</code> where `0 <= i < nums2.length` and `0 <= j < k < nums1.length`.

**Example 1:**

**Input:** nums1 = [7,4], nums2 = [5,2,8,9]

**Output:** 1

**Explanation:** Type 1: (1, 1, 2), nums1[1]<sup>2</sup> = nums2[1] \* nums2[2]. (4<sup>2</sup> = 2 \* 8).

**Example 2:**

**Input:** nums1 = [1,1], nums2 = [1,1,1]

**Output:** 9

**Explanation:** All Triplets are valid, because 1<sup>2</sup> = 1 \* 1.

Type 1: (0,0,1), (0,0,2), (0,1,2), (1,0,1), (1,0,2), (1,1,2). nums1[i]<sup>2</sup> = nums2[j] \* nums2[k].

Type 2: (0,0,1), (1,0,1), (2,0,1). nums2[i]<sup>2</sup> = nums1[j] \* nums1[k].

**Example 3:**

**Input:** nums1 = [7,7,8,3], nums2 = [1,2,9,7]

**Output:** 2

**Explanation:** There are 2 valid triplets.

Type 1: (3,0,2). nums1[3]<sup>2</sup> = nums2[0] \* nums2[2].

Type 2: (3,0,1). nums2[3]<sup>2</sup> = nums1[0] \* nums1[1].

**Constraints:**

*   `1 <= nums1.length, nums2.length <= 1000`
*   <code>1 <= nums1[i], nums2[i] <= 10<sup>5</sup></code>

## Solution

```kotlin
class Solution {
    fun numTriplets(nums1: IntArray, nums2: IntArray): Int {
        nums1.sort()
        nums2.sort()
        return count(nums1, nums2) + count(nums2, nums1)
    }

    fun count(a: IntArray, b: IntArray): Int {
        val m = b.size
        var count = 0
        for (value in a) {
            val x = value.toLong() * value
            var j = 0
            var k = m - 1
            while (j < k) {
                val prod = b[j].toLong() * b[k]
                if (prod < x) {
                    j++
                } else if (prod > x) {
                    k--
                } else if (b[j] != b[k]) {
                    var jNew = j
                    var kNew = k
                    while (b[j] == b[jNew]) {
                        jNew++
                    }
                    while (b[k] == b[kNew]) {
                        kNew--
                    }
                    count += (jNew - j) * (k - kNew)
                    j = jNew
                    k = kNew
                } else {
                    val q = k - j + 1
                    count += q * (q - 1) / 2
                    break
                }
            }
        }
        return count
    }
}
```