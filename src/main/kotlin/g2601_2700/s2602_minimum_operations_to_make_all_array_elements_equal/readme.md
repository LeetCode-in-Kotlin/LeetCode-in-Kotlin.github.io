[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2602\. Minimum Operations to Make All Array Elements Equal

Medium

You are given an array `nums` consisting of positive integers.

You are also given an integer array `queries` of size `m`. For the <code>i<sup>th</sup></code> query, you want to make all of the elements of `nums` equal to `queries[i]`. You can perform the following operation on the array **any** number of times:

*   **Increase** or **decrease** an element of the array by `1`.

Return _an array_ `answer` _of size_ `m` _where_ `answer[i]` _is the **minimum** number of operations to make all elements of_ `nums` _equal to_ `queries[i]`.

**Note** that after each query the array is reset to its original state.

**Example 1:**

**Input:** nums = [3,1,6,8], queries = [1,5]

**Output:** [14,10]

**Explanation:** For the first query we can do the following operations: 
- Decrease nums[0] 2 times, so that nums = [1,1,6,8]. 
- Decrease nums[2] 5 times, so that nums = [1,1,1,8]. 
- Decrease nums[3] 7 times, so that nums = [1,1,1,1]. 

So the total number of operations for the first query is 2 + 5 + 7 = 14. 

For the second query we can do the following operations: 
- Increase nums[0] 2 times, so that nums = [5,1,6,8]. 
- Increase nums[1] 4 times, so that nums = [5,5,6,8]. 
- Decrease nums[2] 1 time, so that nums = [5,5,5,8]. 
- Decrease nums[3] 3 times, so that nums = [5,5,5,5]. 

So the total number of operations for the second query is 2 + 4 + 1 + 3 = 10.

**Example 2:**

**Input:** nums = [2,9,6,3], queries = [10]

**Output:** [20]

**Explanation:** We can increase each value in the array to 10. The total number of operations will be 8 + 1 + 4 + 7 = 20.

**Constraints:**

*   `n == nums.length`
*   `m == queries.length`
*   <code>1 <= n, m <= 10<sup>5</sup></code>
*   <code>1 <= nums[i], queries[i] <= 10<sup>9</sup></code>

## Solution

```kotlin
class Solution {
    fun minOperations(nums: IntArray, queries: IntArray): List<Long> {
        nums.sort()
        val sum = LongArray(nums.size)
        sum[0] = nums[0].toLong()
        for (i in 1 until nums.size) {
            sum[i] = sum[i - 1] + nums[i].toLong()
        }
        val res: MutableList<Long> = ArrayList()
        for (query in queries) {
            res.add(getOperations(sum, nums, query))
        }
        return res
    }

    private fun getOperations(sum: LongArray, nums: IntArray, target: Int): Long {
        var res: Long = 0
        val index = getIndex(nums, target)
        val rightCounts = nums.size - 1 - index
        if (index > 0) {
            res += index.toLong() * target - sum[index - 1]
        }
        if (rightCounts > 0) {
            res += sum[nums.size - 1] - sum[index] - rightCounts.toLong() * target
        }
        res += kotlin.math.abs(target - nums[index]).toLong()
        return res
    }

    private fun getIndex(nums: IntArray, target: Int): Int {
        var index = nums.binarySearch(target)
        if (index < 0) index = -(index + 1)
        if (index == nums.size) --index
        return index
    }
}
```