[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1526\. Minimum Number of Increments on Subarrays to Form a Target Array

Hard

You are given an integer array `target`. You have an integer array `initial` of the same size as `target` with all elements initially zeros.

In one operation you can choose **any** subarray from `initial` and increment each value by one.

Return _the minimum number of operations to form a_ `target` _array from_ `initial`.

The test cases are generated so that the answer fits in a 32-bit integer.

**Example 1:**

**Input:** target = [1,2,3,2,1]

**Output:** 3

**Explanation:** We need at least 3 operations to form the target array from the initial array. 

[**0,0,0,0,0**] increment 1 from index 0 to 4 (inclusive). 

[1,**1,1,1**,1] increment 1 from index 1 to 3 (inclusive). 

[1,2,**2**,2,1] increment 1 at index 2. 

[1,2,3,2,1] target array is formed.

**Example 2:**

**Input:** target = [3,1,1,2]

**Output:** 4

**Explanation:** [**0,0,0,0**] -> [1,1,1,**1**] -> [**1**,1,1,2] -> [**2**,1,1,2] -> [3,1,1,2]

**Example 3:**

**Input:** target = [3,1,5,4,2]

**Output:** 7

**Explanation:** [**0,0,0,0,0**] -> [**1**,1,1,1,1] -> [**2**,1,1,1,1] -> [3,1,**1,1,1**] -> [3,1,**2,2**,2] -> [3,1,**3,3**,2] -> [3,1,**4**,4,2] -> [3,1,5,4,2].

**Constraints:**

*   <code>1 <= target.length <= 10<sup>5</sup></code>
*   <code>1 <= target[i] <= 10<sup>5</sup></code>

## Solution

```kotlin
class Solution {
    fun minNumberOperations(target: IntArray): Int {
        var operations = target[0]
        for (i in 1 until target.size) {
            if (target[i] > target[i - 1]) {
                operations += target[i] - target[i - 1]
            }
        }
        return operations
    }
}
```