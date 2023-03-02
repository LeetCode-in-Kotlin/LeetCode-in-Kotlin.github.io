[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 47\. Permutations II

Medium

Given a collection of numbers, `nums`, that might contain duplicates, return _all possible unique permutations **in any order**._

**Example 1:**

**Input:** nums = [1,1,2]

**Output:** [[1,1,2], [1,2,1], [2,1,1]]

**Example 2:**

**Input:** nums = [1,2,3]

**Output:** [[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]

**Constraints:**

*   `1 <= nums.length <= 8`
*   `-10 <= nums[i] <= 10`

## Solution

```kotlin
class Solution {
    private var ans: MutableList<List<Int>>? = null

    fun permuteUnique(nums: IntArray): List<List<Int>> {
        ans = ArrayList()
        permute(nums, 0)
        return ans as ArrayList<List<Int>>
    }

    private fun permute(nums: IntArray, p: Int) {
        if (p >= nums.size - 1) {
            val t: MutableList<Int> = ArrayList(nums.size)
            for (n in nums) {
                t.add(n)
            }
            ans!!.add(t)
            return
        }
        permute(nums, p + 1)
        val used = BooleanArray(30)
        for (i in p + 1 until nums.size) {
            if (nums[i] != nums[p] && !used[10 + nums[i]]) {
                used[10 + nums[i]] = true
                swap(nums, p, i)
                permute(nums, p + 1)
                swap(nums, p, i)
            }
        }
    }

    private fun swap(nums: IntArray, i: Int, j: Int) {
        val t = nums[i]
        nums[i] = nums[j]
        nums[j] = t
    }
}
```