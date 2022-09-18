[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 40\. Combination Sum II

Medium

Given a collection of candidate numbers (`candidates`) and a target number (`target`), find all unique combinations in `candidates` where the candidate numbers sum to `target`.

Each number in `candidates` may only be used **once** in the combination.

**Note:** The solution set must not contain duplicate combinations.

**Example 1:**

**Input:** candidates = [10,1,2,7,6,1,5], target = 8

**Output:** [ [1,1,6], [1,2,5], [1,7], [2,6] ]

**Example 2:**

**Input:** candidates = [2,5,2,1,2], target = 5

**Output:** [ [1,2,2], [5] ]

**Constraints:**

*   `1 <= candidates.length <= 100`
*   `1 <= candidates[i] <= 50`
*   `1 <= target <= 30`

## Solution

```kotlin
import java.util.Arrays
import java.util.LinkedList

class Solution {
    fun combinationSum2(candidates: IntArray, target: Int): List<List<Int>> {
        val sums: MutableList<List<Int>> = ArrayList()
        // optimize
        Arrays.sort(candidates)
        combinationSum(candidates, target, 0, sums, LinkedList())
        return sums
    }

    private fun combinationSum(
        candidates: IntArray,
        target: Int,
        start: Int,
        sums: MutableList<List<Int>>,
        sum: LinkedList<Int>
    ) {
        if (target == 0) {
            // make a deep copy of the current combination
            sums.add(ArrayList(sum))
            return
        }
        var i = start
        while (i < candidates.size && target >= candidates[i]) {

            // If candidate[i] equals candidate[i-1], then solutions for i is subset of
            // solution of i-1
            if (i == start || i > start && candidates[i] != candidates[i - 1]) {
                sum.addLast(candidates[i])
                // call on 'i+1' (not i) to avoid duplicate usage of same element
                combinationSum(candidates, target - candidates[i], i + 1, sums, sum)
                sum.removeLast()
            }
            i++
        }
    }
}
```