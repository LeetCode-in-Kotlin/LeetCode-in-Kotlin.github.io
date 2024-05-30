[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3159\. Find Occurrences of an Element in an Array

Medium

You are given an integer array `nums`, an integer array `queries`, and an integer `x`.

For each `queries[i]`, you need to find the index of the <code>queries[i]<sup>th</sup></code> occurrence of `x` in the `nums` array. If there are fewer than `queries[i]` occurrences of `x`, the answer should be -1 for that query.

Return an integer array `answer` containing the answers to all queries.

**Example 1:**

**Input:** nums = [1,3,1,7], queries = [1,3,2,4], x = 1

**Output:** [0,-1,2,-1]

**Explanation:**

*   For the 1<sup>st</sup> query, the first occurrence of 1 is at index 0.
*   For the 2<sup>nd</sup> query, there are only two occurrences of 1 in `nums`, so the answer is -1.
*   For the 3<sup>rd</sup> query, the second occurrence of 1 is at index 2.
*   For the 4<sup>th</sup> query, there are only two occurrences of 1 in `nums`, so the answer is -1.

**Example 2:**

**Input:** nums = [1,2,3], queries = [10], x = 5

**Output:** [-1]

**Explanation:**

*   For the 1<sup>st</sup> query, 5 doesn't exist in `nums`, so the answer is -1.

**Constraints:**

*   <code>1 <= nums.length, queries.length <= 10<sup>5</sup></code>
*   <code>1 <= queries[i] <= 10<sup>5</sup></code>
*   <code>1 <= nums[i], x <= 10<sup>4</sup></code>

## Solution

```kotlin
class Solution {
    fun occurrencesOfElement(nums: IntArray, queries: IntArray, x: Int): IntArray {
        val a = ArrayList<Int>()
        run {
            var i = 0
            val l = nums.size
            while (i < l) {
                if (nums[i] == x) {
                    a.add(i)
                }
                i++
            }
        }
        val l = queries.size
        val r = IntArray(l)
        for (i in 0 until l) {
            r[i] = if (queries[i] > a.size) -1 else a[queries[i] - 1]
        }
        return r
    }
}
```