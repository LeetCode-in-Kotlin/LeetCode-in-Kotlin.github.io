[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 78\. Subsets

Medium

Given an integer array `nums` of **unique** elements, return _all possible subsets (the power set)_.

The solution set **must not** contain duplicate subsets. Return the solution in **any order**.

**Example 1:**

**Input:** nums = [1,2,3]

**Output:** [[],[1],[2],[1,2],[3],[1,3],[2,3],[1,2,3]]

**Example 2:**

**Input:** nums = [0]

**Output:** [[],[0]]

**Constraints:**

*   `1 <= nums.length <= 10`
*   `-10 <= nums[i] <= 10`
*   All the numbers of `nums` are **unique**.

## Solution

```kotlin
class Solution {
    fun subsets(nums: IntArray): List<List<Int>> {
        val res: MutableList<List<Int>> = ArrayList()
        solve(nums, ArrayList(), res, 0)
        return res
    }

    private fun solve(nums: IntArray, temp: MutableList<Int>, res: MutableList<List<Int>>, start: Int) {
        res.add(ArrayList(temp))
        for (i in start until nums.size) {
            temp.add(nums[i])
            solve(nums, temp, res, i + 1)
            temp.removeAt(temp.size - 1)
        }
    }
}
```