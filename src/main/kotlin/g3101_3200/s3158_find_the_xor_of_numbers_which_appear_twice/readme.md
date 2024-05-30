[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3158\. Find the XOR of Numbers Which Appear Twice

Easy

You are given an array `nums`, where each number in the array appears **either** once or twice.

Return the bitwise `XOR` of all the numbers that appear twice in the array, or 0 if no number appears twice.

**Example 1:**

**Input:** nums = [1,2,1,3]

**Output:** 1

**Explanation:**

The only number that appears twice in `nums` is 1.

**Example 2:**

**Input:** nums = [1,2,3]

**Output:** 0

**Explanation:**

No number appears twice in `nums`.

**Example 3:**

**Input:** nums = [1,2,2,1]

**Output:** 3

**Explanation:**

Numbers 1 and 2 appeared twice. `1 XOR 2 == 3`.

**Constraints:**

*   `1 <= nums.length <= 50`
*   `1 <= nums[i] <= 50`
*   Each number in `nums` appears either once or twice.

## Solution

```kotlin
class Solution {
    fun duplicateNumbersXOR(nums: IntArray): Int {
        val appeared = BooleanArray(51)
        var res = 0
        for (num in nums) {
            if (appeared[num]) {
                res = res xor num
            }
            appeared[num] = true
        }
        return res
    }
}
```