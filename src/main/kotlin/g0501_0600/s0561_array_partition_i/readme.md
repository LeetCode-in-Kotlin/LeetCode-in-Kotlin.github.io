[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 561\. Array Partition

Easy

Given an integer array `nums` of `2n` integers, group these integers into `n` pairs <code>(a<sub>1</sub>, b<sub>1</sub>), (a<sub>2</sub>, b<sub>2</sub>), ..., (a<sub>n</sub>, b<sub>n</sub>)</code> such that the sum of <code>min(a<sub>i</sub>, b<sub>i</sub>)</code> for all `i` is **maximized**. Return _the maximized sum_.

**Example 1:**

**Input:** nums = [1,4,3,2]

**Output:** 4

**Explanation:** All possible pairings (ignoring the ordering of elements) are:
1. (1, 4), (2, 3) -> min(1, 4) + min(2, 3) = 1 + 2 = 3 
2. (1, 3), (2, 4) -> min(1, 3) + min(2, 4) = 1 + 2 = 3 
3. (1, 2), (3, 4) -> min(1, 2) + min(3, 4) = 1 + 3 = 4 So the maximum possible sum is 4.

**Example 2:**

**Input:** nums = [6,2,6,5,1,2]

**Output:** 9

**Explanation:** The optimal pairing is (2, 1), (2, 5), (6, 6). min(2, 1) + min(2, 5) + min(6, 6) = 1 + 2 + 6 = 9.

**Constraints:**

*   <code>1 <= n <= 10<sup>4</sup></code>
*   `nums.length == 2 * n`
*   <code>-10<sup>4</sup> <= nums[i] <= 10<sup>4</sup></code>

## Solution

```kotlin
class Solution {
    fun arrayPairSum(nums: IntArray): Int {
        nums.sort()
        var sum = 0
        var i = 0
        while (i < nums.size - 1) {
            sum += Math.min(nums[i], nums[i + 1])
            i = i + 2
        }
        return sum
    }
}
```