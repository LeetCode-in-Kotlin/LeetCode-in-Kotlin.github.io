[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 491\. Non-decreasing Subsequences

Medium

Given an integer array `nums`, return _all the different possible non-decreasing subsequences of the given array with at least two elements_. You may return the answer in **any order**.

**Example 1:**

**Input:** nums = [4,6,7,7]

**Output:** [[4,6],[4,6,7],[4,6,7,7],[4,7],[4,7,7],[6,7],[6,7,7],[7,7]]

**Example 2:**

**Input:** nums = [4,4,3,2,1]

**Output:** [[4,4]]

**Constraints:**

*   `1 <= nums.length <= 15`
*   `-100 <= nums[i] <= 100`

## Solution

```kotlin
class Solution {
    fun findSubsequences(nums: IntArray): List<List<Int>> {
        if (nums.size == 1) {
            return ArrayList()
        }
        val answer: MutableSet<List<Int>> = HashSet()
        val list: MutableList<Int> = ArrayList()
        return ArrayList(backtracking(nums, 0, list, answer))
    }

    private fun backtracking(
        nums: IntArray,
        start: Int,
        currList: MutableList<Int>,
        answer: MutableSet<List<Int>>,
    ): Set<List<Int>> {
        if (currList.size >= 2) {
            answer.add(ArrayList(currList))
        }
        for (i in start until nums.size) {
            if (currList.isEmpty() || currList[currList.size - 1] <= nums[i]) {
                currList.add(nums[i])
                backtracking(nums, i + 1, currList, answer)
                currList.removeAt(currList.size - 1)
            }
        }
        return answer
    }
}
```