[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3354\. Make Array Elements Equal to Zero

Easy

You are given an integer array `nums`.

Start by selecting a starting position `curr` such that `nums[curr] == 0`, and choose a movement **direction** of either left or right.

After that, you repeat the following process:

*   If `curr` is out of the range `[0, n - 1]`, this process ends.
*   If `nums[curr] == 0`, move in the current direction by **incrementing** `curr` if you are moving right, or **decrementing** `curr` if you are moving left.
*   Else if `nums[curr] > 0`:
    *   Decrement `nums[curr]` by 1.
    *   **Reverse** your movement direction (left becomes right and vice versa).
    *   Take a step in your new direction.

A selection of the initial position `curr` and movement direction is considered **valid** if every element in `nums` becomes 0 by the end of the process.

Return the number of possible **valid** selections.

**Example 1:**

**Input:** nums = [1,0,2,0,3]

**Output:** 2

**Explanation:**

The only possible valid selections are the following:

*   Choose `curr = 3`, and a movement direction to the left.
    *   <code>[1,0,2,**<ins>0</ins>**,3] -> [1,0,**<ins>2</ins>**,0,3] -> [1,0,1,**<ins>0</ins>**,3] -> [1,0,1,0,**<ins>3</ins>**] -> [1,0,1,**<ins>0</ins>**,2] -> [1,0,**<ins>1</ins>**,0,2] -> [1,0,0,**<ins>0</ins>**,2] -> [1,0,0,0,**<ins>2</ins>**] -> [1,0,0,**<ins>0</ins>**,1] -> [1,0,**<ins>0</ins>**,0,1] -> [1,**<ins>0</ins>**,0,0,1] -> [**<ins>1</ins>**,0,0,0,1] -> [0,**<ins>0</ins>**,0,0,1] -> [0,0,**<ins>0</ins>**,0,1] -> [0,0,0,**<ins>0</ins>**,1] -> [0,0,0,0,**<ins>1</ins>**] -> [0,0,0,0,0]</code>.
*   Choose `curr = 3`, and a movement direction to the right.
    *   <code>[1,0,2,**<ins>0</ins>**,3] -> [1,0,2,0,**<ins>3</ins>**] -> [1,0,2,**<ins>0</ins>**,2] -> [1,0,**<ins>2</ins>**,0,2] -> [1,0,1,**<ins>0</ins>**,2] -> [1,0,1,0,**<ins>2</ins>**] -> [1,0,1,**<ins>0</ins>**,1] -> [1,0,**<ins>1</ins>**,0,1] -> [1,0,0,**<ins>0</ins>**,1] -> [1,0,0,0,**<ins>1</ins>**] -> [1,0,0,**<ins>0</ins>**,0] -> [1,0,**<ins>0</ins>**,0,0] -> [1,**<ins>0</ins>**,0,0,0] -> [**<ins>1</ins>**,0,0,0,0] -> [0,0,0,0,0].</code>

**Example 2:**

**Input:** nums = [2,3,4,0,4,1,0]

**Output:** 0

**Explanation:**

There are no possible valid selections.

**Constraints:**

*   `1 <= nums.length <= 100`
*   `0 <= nums[i] <= 100`
*   There is at least one element `i` where `nums[i] == 0`.

## Solution

```kotlin
import kotlin.math.abs

class Solution {
    fun countValidSelections(nums: IntArray): Int {
        val rightSum = IntArray(nums.size)
        val leftSum = IntArray(nums.size)
        var result = 0
        leftSum[0] = 0
        rightSum[nums.size - 1] = 0
        for (i in 1..<nums.size) {
            leftSum[i] = leftSum[i - 1] + nums[i - 1]
        }
        for (j in nums.size - 2 downTo 0) {
            rightSum[j] = rightSum[j + 1] + nums[j + 1]
        }
        for (k in nums.indices) {
            if (nums[k] == 0 && abs((rightSum[k] - leftSum[k])) == 1) {
                result++
            }
            if (nums[k] == 0 && abs((rightSum[k] - leftSum[k])) == 0) {
                result += 2
            }
        }
        return result
    }
}
```