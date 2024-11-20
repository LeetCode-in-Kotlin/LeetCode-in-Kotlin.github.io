[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2404\. Most Frequent Even Element

Easy

Given an integer array `nums`, return _the most frequent even element_.

If there is a tie, return the **smallest** one. If there is no such element, return `-1`.

**Example 1:**

**Input:** nums = [0,1,2,2,4,4,1]

**Output:** 2

**Explanation:**

The even elements are 0, 2, and 4. Of these, 2 and 4 appear the most.

We return the smallest one, which is 2.

**Example 2:**

**Input:** nums = [4,4,4,9,2,4]

**Output:** 4

**Explanation:** 4 is the even element appears the most. 

**Example 3:**

**Input:** nums = [29,47,21,41,13,37,25,7]

**Output:** -1

**Explanation:** There is no even element. 

**Constraints:**

*   `1 <= nums.length <= 2000`
*   <code>0 <= nums[i] <= 10<sup>5</sup></code>

## Solution

```kotlin
class Solution {
    fun mostFrequentEven(nums: IntArray): Int {
        val hm = HashMap<Int, Int>()
        var max = 0
        var small = Int.MAX_VALUE

        if (nums.size == 1) {
            return if (nums[0] % 2 == 0) {
                nums[0]
            } else {
                -1
            }
        }

        for (i in nums.indices) {
            if (nums[i] % 2 == 0) {
                hm[nums[i]] = hm.getOrDefault(nums[i], 0) + 1
                if (hm[nums[i]]!! > max) {
                    max = hm[nums[i]]!!
                }
            }
        }

        for ((key, value) in hm) {
            if (value == max && key < small) {
                small = key
            }
        }

        return if (small == Int.MAX_VALUE) -1 else small
    }
}
```