[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 496\. Next Greater Element I

Easy

The **next greater element** of some element `x` in an array is the **first greater** element that is **to the right** of `x` in the same array.

You are given two **distinct 0-indexed** integer arrays `nums1` and `nums2`, where `nums1` is a subset of `nums2`.

For each `0 <= i < nums1.length`, find the index `j` such that `nums1[i] == nums2[j]` and determine the **next greater element** of `nums2[j]` in `nums2`. If there is no next greater element, then the answer for this query is `-1`.

Return _an array_ `ans` _of length_ `nums1.length` _such that_ `ans[i]` _is the **next greater element** as described above._

**Example 1:**

**Input:** nums1 = [4,1,2], nums2 = [1,3,4,2]

**Output:** [-1,3,-1]

**Explanation:** The next greater element for each value of nums1 is as follows: 

- 4 is underlined in nums2 = [1,3,<ins>4</ins>,2]. There is no next greater element, so the answer is -1. 

- 1 is underlined in nums2 = [<ins>1</ins>,3,4,2]. The next greater element is 3. 

- 2 is underlined in nums2 = [1,3,4,<ins>2</ins>]. There is no next greater element, so the answer is -1.

**Example 2:**

**Input:** nums1 = [2,4], nums2 = [1,2,3,4]

**Output:** [3,-1]

**Explanation:** The next greater element for each value of nums1 is as follows: 

- 2 is underlined in nums2 = [1,<ins>2</ins>,3,4]. The next greater element is 3. 

- 4 is underlined in nums2 = [1,2,3,<ins>4</ins>]. There is no next greater element, so the answer is -1.

**Constraints:**

*   `1 <= nums1.length <= nums2.length <= 1000`
*   <code>0 <= nums1[i], nums2[i] <= 10<sup>4</sup></code>
*   All integers in `nums1` and `nums2` are **unique**.
*   All the integers of `nums1` also appear in `nums2`.

**Follow up:** Could you find an `O(nums1.length + nums2.length)` solution?

## Solution

```kotlin
class Solution {
    fun nextGreaterElement(nums1: IntArray, nums2: IntArray): IntArray {
        val indexMap: MutableMap<Int, Int> = HashMap()
        for (i in nums2.indices) {
            indexMap[nums2[i]] = i
        }
        for (i in nums1.indices) {
            val num = nums1[i]
            var index = indexMap[num]!!
            if (index == nums2.size - 1) {
                nums1[i] = -1
            } else {
                var found = false
                while (index < nums2.size) {
                    if (nums2[index] > num) {
                        nums1[i] = nums2[index]
                        found = true
                        break
                    }
                    index++
                }
                if (!found) {
                    nums1[i] = -1
                }
            }
        }
        return nums1
    }
}
```