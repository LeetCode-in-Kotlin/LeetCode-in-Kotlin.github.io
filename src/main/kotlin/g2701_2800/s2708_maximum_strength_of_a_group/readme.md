[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2708\. Maximum Strength of a Group

Medium

You are given a **0-indexed** integer array `nums` representing the score of students in an exam. The teacher would like to form one **non-empty** group of students with maximal **strength**, where the strength of a group of students of indices <code>i<sub>0</sub></code>, <code>i<sub>1</sub></code>, <code>i<sub>2</sub></code>, ... , <code>i<sub>k</sub></code> is defined as <code>nums[i<sub>0</sub>] * nums[i<sub>1</sub>] * nums[i<sub>2</sub>] * ... * nums[i<sub>k</sub>]</code>.

Return _the maximum strength of a group the teacher can create_.

**Example 1:**

**Input:** nums = [3,-1,-5,2,5,-9]

**Output:** 1350

**Explanation:** One way to form a group of maximal strength is to group the students at indices [0,2,3,4,5]. Their strength is 3 \* (-5) \* 2 \* 5 \* (-9) = 1350, which we can show is optimal.

**Example 2:**

**Input:** nums = [-4,-5,-4]

**Output:** 20

**Explanation:** Group the students at indices [0, 1] . Then, we’ll have a resulting strength of 20. We cannot achieve greater strength.

**Constraints:**

*   `1 <= nums.length <= 13`
*   `-9 <= nums[i] <= 9`

## Solution

```kotlin
class Solution {
    fun maxStrength(nums: IntArray): Long {
        val filtered = mutableListOf<Int>()
        var product = 1L
        var hasZero = false
        for (num in nums) {
            if (num == 0) {
                hasZero = true
                continue
            }
            filtered.add(num)
            product *= num.toLong()
        }
        if (filtered.isEmpty()) return 0
        if (filtered.size == 1 && filtered[0] <= 0) return if (hasZero) 0 else filtered[0].toLong()
        var result = product
        for (num in nums) {
            if (num == 0) continue
            result = result.coerceAtLeast(product / num.toLong())
        }
        return result
    }
}
```