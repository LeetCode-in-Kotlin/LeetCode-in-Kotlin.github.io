[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1262\. Greatest Sum Divisible by Three

Medium

Given an array `nums` of integers, we need to find the maximum possible sum of elements of the array such that it is divisible by three.

**Example 1:**

**Input:** nums = [3,6,5,1,8]

**Output:** 18

**Explanation:** Pick numbers 3, 6, 1 and 8 their sum is 18 (maximum sum divisible by 3).

**Example 2:**

**Input:** nums = [4]

**Output:** 0

**Explanation:** Since 4 is not divisible by 3, do not pick any number. 

**Example 3:**

**Input:** nums = [1,2,3,4,4]

**Output:** 12

**Explanation:** Pick numbers 1, 3, 4 and 4 their sum is 12 (maximum sum divisible by 3). 

**Constraints:**

*   `1 <= nums.length <= 4 * 10^4`
*   `1 <= nums[i] <= 10^4`

## Solution

```kotlin
class Solution {
    fun maxSumDivThree(nums: IntArray): Int {
        var sum = 0
        var smallestNumWithMod1 = 10001
        var secondSmallestNumWithMod1 = 10002
        var smallestNumWithMod2 = 10001
        var secondSmallestNumWithMod2 = 10002
        for (i in nums) {
            sum += i
            if (i % 3 == 1) {
                if (i <= smallestNumWithMod1) {
                    val temp = smallestNumWithMod1
                    smallestNumWithMod1 = i
                    secondSmallestNumWithMod1 = temp
                } else if (i < secondSmallestNumWithMod1) {
                    secondSmallestNumWithMod1 = i
                }
            }
            if (i % 3 == 2) {
                if (i <= smallestNumWithMod2) {
                    val temp = smallestNumWithMod2
                    smallestNumWithMod2 = i
                    secondSmallestNumWithMod2 = temp
                } else if (i < secondSmallestNumWithMod2) {
                    secondSmallestNumWithMod2 = i
                }
            }
        }
        if (sum % 3 == 0) {
            return sum
        } else if (sum % 3 == 2) {
            val min = Math.min(smallestNumWithMod2, smallestNumWithMod1 + secondSmallestNumWithMod1)
            return sum - min
        } else if (sum % 3 == 1) {
            val min = Math.min(smallestNumWithMod1, smallestNumWithMod2 + secondSmallestNumWithMod2)
            return sum - min
        }
        return sum
    }
}
```