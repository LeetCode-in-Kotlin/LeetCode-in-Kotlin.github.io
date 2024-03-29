[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1299\. Replace Elements with Greatest Element on Right Side

Easy

Given an array `arr`, replace every element in that array with the greatest element among the elements to its right, and replace the last element with `-1`.

After doing so, return the array.

**Example 1:**

**Input:** arr = [17,18,5,4,6,1]

**Output:** [18,6,6,6,1,-1]

**Explanation:**

- index 0 --> the greatest element to the right of index 0 is index 1 (18).

- index 1 --> the greatest element to the right of index 1 is index 4 (6).

- index 2 --> the greatest element to the right of index 2 is index 4 (6).

- index 3 --> the greatest element to the right of index 3 is index 4 (6).

- index 4 --> the greatest element to the right of index 4 is index 5 (1).

- index 5 --> there are no elements to the right of index 5, so we put -1.

**Example 2:**

**Input:** arr = [400]

**Output:** [-1]

**Explanation:** There are no elements to the right of index 0.

**Constraints:**

*   <code>1 <= arr.length <= 10<sup>4</sup></code>
*   <code>1 <= arr[i] <= 10<sup>5</sup></code>

## Solution

```kotlin
class Solution {
    fun replaceElements(arr: IntArray): IntArray {
        var max = -1
        for (i in arr.indices.reversed()) {
            val temp = arr[i]
            arr[i] = max
            max = Math.max(max, temp)
        }
        return arr
    }
}
```