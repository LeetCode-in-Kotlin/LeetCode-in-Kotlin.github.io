[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1186\. Maximum Subarray Sum with One Deletion

Medium

Given an array of integers, return the maximum sum for a **non-empty** subarray (contiguous elements) with at most one element deletion. In other words, you want to choose a subarray and optionally delete one element from it so that there is still at least one element left and the sum of the remaining elements is maximum possible.

Note that the subarray needs to be **non-empty** after deleting one element.

**Example 1:**

**Input:** arr = [1,-2,0,3]

**Output:** 4

**Explanation:** Because we can choose [1, -2, 0, 3] and drop -2, thus the subarray [1, 0, 3] becomes the maximum value.

**Example 2:**

**Input:** arr = [1,-2,-2,3]

**Output:** 3

**Explanation:** We just choose [3] and it's the maximum sum.

**Example 3:**

**Input:** arr = [-1,-1,-1,-1]

**Output:** -1

**Explanation:** The final subarray needs to be non-empty. You can't choose [-1] and delete -1 from it, then get an empty subarray to make the sum equals to 0.

**Constraints:**

*   <code>1 <= arr.length <= 10<sup>5</sup></code>
*   <code>-10<sup>4</sup> <= arr[i] <= 10<sup>4</sup></code>

## Solution

```kotlin
class Solution {
    fun maximumSum(arr: IntArray): Int {
        var maxWithNoDeletions = arr[0]
        var maxWithOneDeletion = arr[0]
        var maxOverall = arr[0]
        for (i in 1 until arr.size) {
            val numToProcess = arr[i]
            val nextMaxWithNoDeletions = Math.max(maxWithNoDeletions + numToProcess, numToProcess)
            val nextMaxWithOneDeletion = Math.max(maxWithOneDeletion + numToProcess, maxWithNoDeletions)
            maxOverall = Math.max(maxOverall, Math.max(nextMaxWithNoDeletions, nextMaxWithOneDeletion))
            maxWithNoDeletions = nextMaxWithNoDeletions
            maxWithOneDeletion = nextMaxWithOneDeletion
        }
        return maxOverall
    }
}
```