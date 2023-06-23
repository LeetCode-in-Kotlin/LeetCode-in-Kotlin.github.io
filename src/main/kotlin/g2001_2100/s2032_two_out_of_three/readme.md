[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2032\. Two Out of Three

Easy

Given three integer arrays `nums1`, `nums2`, and `nums3`, return _a **distinct** array containing all the values that are present in **at least two** out of the three arrays. You may return the values in **any** order_.

**Example 1:**

**Input:** nums1 = [1,1,3,2], nums2 = [2,3], nums3 = [3]

**Output:** [3,2]

**Explanation:** The values that are present in at least two arrays are: 

- 3, in all three arrays. 

- 2, in nums1 and nums2.

**Example 2:**

**Input:** nums1 = [3,1], nums2 = [2,3], nums3 = [1,2]

**Output:** [2,3,1]

**Explanation:** The values that are present in at least two arrays are: 

- 2, in nums2 and nums3.

- 3, in nums1 and nums2. 

- 1, in nums1 and nums3.

**Example 3:**

**Input:** nums1 = [1,2,2], nums2 = [4,3,3], nums3 = [5]

**Output:** []

**Explanation:** No value is present in at least two arrays.

**Constraints:**

*   `1 <= nums1.length, nums2.length, nums3.length <= 100`
*   `1 <= nums1[i], nums2[j], nums3[k] <= 100`

## Solution

```kotlin
class Solution {
    fun twoOutOfThree(nums1: IntArray, nums2: IntArray, nums3: IntArray): List<Int> {
        val ans: MutableSet<Int> = HashSet()
        val set1: MutableSet<Int> = HashSet()
        for (i in nums1) {
            set1.add(i)
        }
        val set2: MutableSet<Int> = HashSet()
        for (i in nums2) {
            set2.add(i)
        }
        val set3: MutableSet<Int> = HashSet()
        for (i in nums3) {
            set3.add(i)
        }
        for (j in nums1) {
            if (set2.contains(j) || set3.contains(j)) {
                ans.add(j)
            }
        }
        for (j in nums2) {
            if (set1.contains(j) || set3.contains(j)) {
                ans.add(j)
            }
        }
        for (j in nums3) {
            if (set1.contains(j) || set2.contains(j)) {
                ans.add(j)
            }
        }
        val result: MutableList<Int> = ArrayList()
        result.addAll(ans)
        return result
    }
}
```