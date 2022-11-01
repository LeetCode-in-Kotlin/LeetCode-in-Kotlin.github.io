[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 229\. Majority Element II

Medium

Given an integer array of size `n`, find all elements that appear more than `⌊ n/3 ⌋` times.

**Example 1:**

**Input:** nums = [3,2,3]

**Output:** [3]

**Example 2:**

**Input:** nums = [1]

**Output:** [1]

**Example 3:**

**Input:** nums = [1,2]

**Output:** [1,2]

**Constraints:**

*   <code>1 <= nums.length <= 5 * 10<sup>4</sup></code>
*   <code>-10<sup>9</sup> <= nums[i] <= 10<sup>9</sup></code>

**Follow up:** Could you solve the problem in linear time and in `O(1)` space?

## Solution

```kotlin
class Solution {
    fun majorityElement(nums: IntArray): List<Int> {
        val results: MutableList<Int> = ArrayList()
        val len = nums.size
        var first = 0
        var second = 1
        var count1 = 0
        var count2 = 0
        for (temp in nums) {
            if (temp == first) {
                count1++
            } else if (temp == second) {
                count2++
            } else if (count1 == 0) {
                first = temp
                count1++
            } else if (count2 == 0) {
                second = temp
                count2++
            } else {
                count1--
                count2--
            }
        }
        count1 = 0
        count2 = 0
        for (temp in nums) {
            if (temp == first) {
                count1++
            }
            if (temp == second) {
                count2++
            }
        }
        if (count1 > len / 3) {
            results.add(first)
        }
        if (count2 > len / 3) {
            results.add(second)
        }
        return results
    }
}
```