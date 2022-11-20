[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 90\. Subsets II

Medium

Given an integer array `nums` that may contain duplicates, return _all possible subsets (the power set)_.

The solution set **must not** contain duplicate subsets. Return the solution in **any order**.

**Example 1:**

**Input:** nums = [1,2,2]

**Output:** [[],[1],[1,2],[1,2,2],[2],[2,2]]

**Example 2:**

**Input:** nums = [0]

**Output:** [[],[0]]

**Constraints:**

*   `1 <= nums.length <= 10`
*   `-10 <= nums[i] <= 10`

## Solution

```kotlin
class Solution {
    var allComb: MutableList<List<Int>> = ArrayList()
    var comb: MutableList<Int> = ArrayList()
    lateinit var nums: IntArray
    fun subsetsWithDup(nums: IntArray): List<List<Int>> {
        nums.sort()
        this.nums = nums
        dfs(0)
        allComb.add(ArrayList())
        return allComb
    }

    private fun dfs(start: Int) {
        if (start > nums.size) {
            return
        }
        for (i in start until nums.size) {
            if (i > start && nums[i] == nums[i - 1]) {
                continue
            }
            comb.add(nums[i])
            allComb.add(ArrayList(comb))
            dfs(i + 1)
            comb.removeAt(comb.size - 1)
        }
    }
}
```