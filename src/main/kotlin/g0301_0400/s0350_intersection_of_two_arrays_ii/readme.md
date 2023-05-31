[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 350\. Intersection of Two Arrays II

Easy

Given two integer arrays `nums1` and `nums2`, return _an array of their intersection_. Each element in the result must appear as many times as it shows in both arrays and you may return the result in **any order**.

**Example 1:**

**Input:** nums1 = [1,2,2,1], nums2 = [2,2]

**Output:** [2,2]

**Example 2:**

**Input:** nums1 = [4,9,5], nums2 = [9,4,9,8,4]

**Output:** [4,9]

**Explanation:** [9,4] is also accepted.

**Constraints:**

*   `1 <= nums1.length, nums2.length <= 1000`
*   `0 <= nums1[i], nums2[i] <= 1000`

**Follow up:**

*   What if the given array is already sorted? How would you optimize your algorithm?
*   What if `nums1`'s size is small compared to `nums2`'s size? Which algorithm is better?
*   What if elements of `nums2` are stored on disk, and the memory is limited such that you cannot load all elements into the memory at once?

## Solution

```kotlin
class Solution {
    fun intersect(nums1: IntArray, nums2: IntArray): IntArray {
        val a: MutableMap<Int, Int> = mutableMapOf()
        for (i in 0 until nums1.size) {
            a[nums1[i]] = a.getOrDefault(nums1[i], 0) + 1
        }
        var s = MutableList<Int>(0) { 0 }
        for (i in 0 until nums2.size) {
            if (a.getOrDefault(nums2[i], 0)> 0) {
                s.add(nums2[i])
                a[nums2[i]] = a.getValue(nums2[i]) - 1
            }
        }
        return s.toIntArray()
    }
}
```