[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2248\. Intersection of Multiple Arrays

Easy

Given a 2D integer array `nums` where `nums[i]` is a non-empty array of **distinct** positive integers, return _the list of integers that are present in **each array** of_ `nums` _sorted in **ascending order**_.

**Example 1:**

**Input:** nums = \[\[**3**,1,2,**4**,5],[1,2,**3**,**4**],[**3**,**4**,5,6]]

**Output:** [3,4]

**Explanation:** 

The only integers present in each of nums[0] = [**3**,1,2,**4**,5], nums[1] = [1,2,**3**,**4**], and nums[2] = [**3**,**4**,5,6] are 3 and 4, so we return [3,4].

**Example 2:**

**Input:** nums = \[\[1,2,3],[4,5,6]]

**Output:** []

**Explanation:** 

There does not exist any integer present both in nums[0] and nums[1], so we return an empty list [].

**Constraints:**

* `1 <= nums.length <= 1000`
* `1 <= sum(nums[i].length) <= 1000`
* `1 <= nums[i][j] <= 1000`
* All the values of `nums[i]` are **unique**.

## Solution

```kotlin
class Solution {
    fun intersection(nums: Array<IntArray>): List<Int> {
        val ans: MutableList<Int> = ArrayList()
        val count = IntArray(1001)
        for (arr in nums) {
            for (i in arr) {
                ++count[i]
            }
        }
        for (i in count.indices) {
            if (count[i] == nums.size) {
                ans.add(i)
            }
        }
        return ans
    }
}
```