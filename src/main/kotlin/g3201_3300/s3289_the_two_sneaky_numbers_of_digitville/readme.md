[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3289\. The Two Sneaky Numbers of Digitville

Easy

In the town of Digitville, there was a list of numbers called `nums` containing integers from `0` to `n - 1`. Each number was supposed to appear **exactly once** in the list, however, **two** mischievous numbers sneaked in an _additional time_, making the list longer than usual.

As the town detective, your task is to find these two sneaky numbers. Return an array of size **two** containing the two numbers (in _any order_), so peace can return to Digitville.

**Example 1:**

**Input:** nums = [0,1,1,0]

**Output:** [0,1]

**Explanation:**

The numbers 0 and 1 each appear twice in the array.

**Example 2:**

**Input:** nums = [0,3,2,1,3,2]

**Output:** [2,3]

**Explanation:**

The numbers 2 and 3 each appear twice in the array.

**Example 3:**

**Input:** nums = [7,1,5,4,3,4,6,0,9,5,8,2]

**Output:** [4,5]

**Explanation:**

The numbers 4 and 5 each appear twice in the array.

**Constraints:**

*   `2 <= n <= 100`
*   `nums.length == n + 2`
*   `0 <= nums[i] < n`
*   The input is generated such that `nums` contains **exactly** two repeated elements.

## Solution

```kotlin
import java.util.HashMap

class Solution {
    fun getSneakyNumbers(nums: IntArray): IntArray {
        val countMap: MutableMap<Int, Int> = HashMap<Int, Int>()
        // Populate the HashMap with the frequency of each number
        for (num in nums) {
            countMap.put(num, countMap.getOrDefault(num, 0) + 1)
        }
        // Array to store the result
        val result = IntArray(2)
        var index = 0
        // Find the numbers that appear exactly twice
        for (entry in countMap.entries) {
            if (entry.value == 2) {
                result[index++] = entry.key
                // Break if we have found both sneaky numbers
                if (index == 2) {
                    break
                }
            }
        }
        return result
    }
}
```