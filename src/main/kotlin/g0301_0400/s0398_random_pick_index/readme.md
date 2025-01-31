[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 398\. Random Pick Index

Medium

Given an integer array `nums` with possible **duplicates**, randomly output the index of a given `target` number. You can assume that the given target number must exist in the array.

Implement the `Solution` class:

*   `Solution(int[] nums)` Initializes the object with the array `nums`.
*   `int pick(int target)` Picks a random index `i` from `nums` where `nums[i] == target`. If there are multiple valid i's, then each index should have an equal probability of returning.

**Example 1:**

**Input**

    ["Solution", "pick", "pick", "pick"] 
    [[[1, 2, 3, 3, 3]], [3], [1], [3]]

**Output:** [null, 4, 0, 2]

**Explanation:**

    Solution solution = new Solution([1, 2, 3, 3, 3]); 
    solution.pick(3); // It should return either index 2, 3, or 4 randomly. Each index should have equal probability of returning. 
    solution.pick(1); // It should return 0. Since in the array only nums[0] is equal to 1. 
    solution.pick(3); // It should return either index 2, 3, or 4 randomly. Each index should have equal probability of returning.

**Constraints:**

*   <code>1 <= nums.length <= 2 * 10<sup>4</sup></code>
*   <code>-2<sup>31</sup> <= nums[i] <= 2<sup>31</sup> - 1</code>
*   `target` is an integer from `nums`.
*   At most <code>10<sup>4</sup></code> calls will be made to `pick`.

## Solution

```kotlin
import kotlin.random.Random

@Suppress("kotlin:S2245")
class Solution(nums: IntArray) {
    // O(n) time | O(n) space
    private val map: MutableMap<Int, MutableList<Int>>

    init {
        map = HashMap()
        for (i in nums.indices) {
            map.computeIfAbsent(
                nums[i],
            ) { ArrayList() }.add(i)
        }
    }

    fun pick(target: Int): Int {
        val list: List<Int> = map[target]!!
        return list[Random.nextInt(list.size)]
    }
}

/*
 * Your Solution object will be instantiated and called as such:
 * var obj = Solution(nums)
 * var param_1 = obj.pick(target)
 */
```