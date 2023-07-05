[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2449\. Minimum Number of Operations to Make Arrays Similar

Hard

You are given two positive integer arrays `nums` and `target`, of the same length.

In one operation, you can choose any two **distinct** indices `i` and `j` where `0 <= i, j < nums.length` and:

*   set `nums[i] = nums[i] + 2` and
*   set `nums[j] = nums[j] - 2`.

Two arrays are considered to be **similar** if the frequency of each element is the same.

Return _the minimum number of operations required to make_ `nums` _similar to_ `target`. The test cases are generated such that `nums` can always be similar to `target`.

**Example 1:**

**Input:** nums = [8,12,6], target = [2,14,10]

**Output:** 2

**Explanation:** It is possible to make nums similar to target in two operations: 

- Choose i = 0 and j = 2, nums = [10,12,4]. 

- Choose i = 1 and j = 2, nums = [10,14,2]. 

It can be shown that 2 is the minimum number of operations needed.

**Example 2:**

**Input:** nums = [1,2,5], target = [4,1,3]

**Output:** 1

**Explanation:** We can make nums similar to target in one operation: - Choose i = 1 and j = 2, nums = [1,4,3].

**Example 3:**

**Input:** nums = [1,1,1,1,1], target = [1,1,1,1,1]

**Output:** 0

**Explanation:** The array nums is already similiar to target.

**Constraints:**

*   `n == nums.length == target.length`
*   <code>1 <= n <= 10<sup>5</sup></code>
*   <code>1 <= nums[i], target[i] <= 10<sup>6</sup></code>
*   It is possible to make `nums` similar to `target`.

## Solution

```kotlin
class Solution {
    fun makeSimilar(nums: IntArray, target: IntArray): Long {
        val evenNums = ArrayList<Int>()
        val oddNums = ArrayList<Int>()
        val evenTar = ArrayList<Int>()
        val oddTar = ArrayList<Int>()
        nums.sort()
        target.sort()
        for (i in nums.indices) {
            if (nums[i] % 2 == 0) {
                evenNums.add(nums[i])
            } else {
                oddNums.add(nums[i])
            }
            if (target[i] % 2 == 0) {
                evenTar.add(target[i])
            } else {
                oddTar.add(target[i])
            }
        }
        var countPositiveIteration: Long = 0
        var countNegativeIteration: Long = 0
        for (i in evenNums.indices) {
            val num = evenNums[i]
            val tar = evenTar[i]
            val diff = num.toLong() - tar
            val iteration = diff / 2
            if (diff > 0) {
                countNegativeIteration += iteration
            } else if (diff < 0) {
                countPositiveIteration += Math.abs(iteration)
            }
        }
        for (i in oddNums.indices) {
            val num = oddNums[i]
            val tar = oddTar[i]
            val diff = num.toLong() - tar
            val iteration = diff / 2
            if (diff > 0) {
                countNegativeIteration += iteration
            } else if (diff < 0) {
                countPositiveIteration += Math.abs(iteration)
            }
        }
        val totalDifference = countPositiveIteration - countNegativeIteration
        return if (totalDifference == 0L) countPositiveIteration else countPositiveIteration + Math.abs(totalDifference)
    }
}
```