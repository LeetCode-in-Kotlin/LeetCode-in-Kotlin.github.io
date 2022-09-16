[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 46\. Permutations

Medium

Given an array `nums` of distinct integers, return _all the possible permutations_. You can return the answer in **any order**.

**Example 1:**

**Input:** nums = [1,2,3]

**Output:** [[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]

**Example 2:**

**Input:** nums = [0,1]

**Output:** [[0,1],[1,0]]

**Example 3:**

**Input:** nums = [1]

**Output:** [[1]]

**Constraints:**

*   `1 <= nums.length <= 6`
*   `-10 <= nums[i] <= 10`
*   All the integers of `nums` are **unique**.

## Solution

```kotlin
class Solution {
    fun permute(nums: IntArray): List<List<Int>> {
        if (nums.isEmpty()) {
            return ArrayList()
        }
        val finalResult: MutableList<List<Int>> = ArrayList()
        permuteRecur(nums, finalResult, ArrayList(), BooleanArray(nums.size))
        return finalResult
    }

    private fun permuteRecur(
        nums: IntArray,
        finalResult: MutableList<List<Int>>,
        currResult: MutableList<Int>,
        used: BooleanArray
    ) {
        if (currResult.size == nums.size) {
            finalResult.add(ArrayList(currResult))
            return
        }
        for (i in nums.indices) {
            if (used[i]) {
                continue
            }
            currResult.add(nums[i])
            used[i] = true
            permuteRecur(nums, finalResult, currResult, used)
            used[i] = false
            currResult.removeAt(currResult.size - 1)
        }
    }
}
```