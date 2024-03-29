[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 985\. Sum of Even Numbers After Queries

Medium

You are given an integer array `nums` and an array `queries` where <code>queries[i] = [val<sub>i</sub>, index<sub>i</sub>]</code>.

For each query `i`, first, apply <code>nums[index<sub>i</sub>] = nums[index<sub>i</sub>] + val<sub>i</sub></code>, then print the sum of the even values of `nums`.

Return _an integer array_ `answer` _where_ `answer[i]` _is the answer to the_ <code>i<sup>th</sup></code> _query_.

**Example 1:**

**Input:** nums = [1,2,3,4], queries = \[\[1,0],[-3,1],[-4,0],[2,3]]

**Output:** [8,6,2,4]

**Explanation:** At the beginning, the array is [1,2,3,4]. 

After adding 1 to nums[0], the array is [2,2,3,4], and the sum of even values is 2 + 2 + 4 = 8. 

After adding -3 to nums[1], the array is [2,-1,3,4], and the sum of even values is 2 + 4 = 6.

After adding -4 to nums[0], the array is [-2,-1,3,4], and the sum of even values is -2 + 4 = 2. 

After adding 2 to nums[3], the array is [-2,-1,3,6], and the sum of even values is -2 + 6 = 4.

**Example 2:**

**Input:** nums = [1], queries = \[\[4,0]]

**Output:** [0]

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>4</sup></code>
*   <code>-10<sup>4</sup> <= nums[i] <= 10<sup>4</sup></code>
*   <code>1 <= queries.length <= 10<sup>4</sup></code>
*   <code>-10<sup>4</sup> <= val<sub>i</sub> <= 10<sup>4</sup></code>
*   <code>0 <= index<sub>i</sub> < nums.length</code>

## Solution

```kotlin
class Solution {
    fun sumEvenAfterQueries(nums: IntArray, queries: Array<IntArray>): IntArray {
        val result = IntArray(queries.size)
        var res = 0
        for (num in nums) {
            res += if (num and 1 == 0) num else 0
        }
        for ((k, query) in queries.withIndex()) {
            res -= if (nums[query[1]] and 1 == 0) nums[query[1]] else 0
            nums[query[1]] += query[0]
            if (nums[query[1]] and 1 == 0) {
                res += nums[query[1]]
            }
            result[k] = res
        }
        return result
    }
}
```