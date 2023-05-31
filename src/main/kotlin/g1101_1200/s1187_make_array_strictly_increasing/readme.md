[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1187\. Make Array Strictly Increasing

Hard

Given two integer arrays `arr1` and `arr2`, return the minimum number of operations (possibly zero) needed to make `arr1` strictly increasing.

In one operation, you can choose two indices `0 <= i < arr1.length` and `0 <= j < arr2.length` and do the assignment `arr1[i] = arr2[j]`.

If there is no way to make `arr1` strictly increasing, return `-1`.

**Example 1:**

**Input:** arr1 = [1,5,3,6,7], arr2 = [1,3,2,4]

**Output:** 1

**Explanation:** Replace `5` with `2`, then `arr1 = [1, 2, 3, 6, 7]`.

**Example 2:**

**Input:** arr1 = [1,5,3,6,7], arr2 = [4,3,1]

**Output:** 2

**Explanation:** Replace `5` with `3` and then replace `3` with `4`. `arr1 = [1, 3, 4, 6, 7]`.

**Example 3:**

**Input:** arr1 = [1,5,3,6,7], arr2 = [1,6,3,3]

**Output:** -1

**Explanation:** You can't make `arr1` strictly increasing.

**Constraints:**

*   `1 <= arr1.length, arr2.length <= 2000`
*   `0 <= arr1[i], arr2[i] <= 10^9`

## Solution

```kotlin
class Solution {
    fun makeArrayIncreasing(arr1: IntArray, arr2: IntArray): Int {
        arr2.sort()
        var start = 0
        for (i in arr2.indices) {
            if (arr2[i] != arr2[start]) {
                arr2[++start] = arr2[i]
            }
        }
        val l2 = start + 1
        val dp = IntArray(l2 + 2)
        for (i in arr1.indices) {
            var noChange = dp[dp.size - 1]
            if (i > 0 && arr1[i - 1] >= arr1[i]) {
                noChange = -1
            }
            for (j in dp.size - 2 downTo 1) {
                if (arr2[j - 1] < arr1[i] && dp[j] != -1) {
                    noChange = if (noChange == -1) dp[j] else Math.min(noChange, dp[j])
                }
                if (dp[j - 1] != -1) {
                    dp[j] = 1 + dp[j - 1]
                } else {
                    dp[j] = -1
                }
                if (i > 0 && arr1[i - 1] < arr2[j - 1] && dp[dp.size - 1] >= 0) {
                    dp[j] = if (dp[j] == -1) dp[dp.size - 1] + 1 else Math.min(dp[j], dp[dp.size - 1] + 1)
                }
            }
            dp[0] = -1
            dp[dp.size - 1] = noChange
        }
        var res = -1
        for (num in dp) {
            if (num != -1) {
                res = if (res == -1) num else Math.min(res, num)
            }
        }
        return res
    }
}
```