[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1995\. Count Special Quadruplets

Easy

Given a **0-indexed** integer array `nums`, return _the number of **distinct** quadruplets_ `(a, b, c, d)` _such that:_

*   `nums[a] + nums[b] + nums[c] == nums[d]`, and
*   `a < b < c < d`

**Example 1:**

**Input:** nums = [1,2,3,6]

**Output:** 1

**Explanation:** The only quadruplet that satisfies the requirement is (0, 1, 2, 3) because 1 + 2 + 3 == 6. 

**Example 2:**

**Input:** nums = [3,3,6,4,5]

**Output:** 0

**Explanation:** There are no such quadruplets in [3,3,6,4,5]. 

**Example 3:**

**Input:** nums = [1,1,1,3,5]

**Output:** 4

**Explanation:** The 4 quadruplets that satisfy the requirement are:

- (0, 1, 2, 3): 1 + 1 + 1 == 3

- (0, 1, 3, 4): 1 + 1 + 3 == 5

- (0, 2, 3, 4): 1 + 1 + 3 == 5

- (1, 2, 3, 4): 1 + 1 + 3 == 5 

**Constraints:**

*   `4 <= nums.length <= 50`
*   `1 <= nums[i] <= 100`

## Solution

```kotlin
class Solution {
    fun countQuadruplets(nums: IntArray): Int {
        var count = 0
        // max nums value is 100 so two elements sum can be max 200
        val m = IntArray(201)
        for (i in 1 until nums.size - 2) {
            for (j in 0 until i) {
                // update all possible 2 sums
                m[nums[j] + nums[i]]++
            }
            for (j in i + 2 until nums.size) {
                // fix third element and search for fourth - third in 2 sums as a  + b + c = d == a
                // +  b = d - c
                val diff = nums[j] - nums[i + 1]
                if (diff >= 0) {
                    count += m[diff]
                }
            }
        }
        return count
    }
}
```