[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 978\. Longest Turbulent Subarray

Medium

Given an integer array `arr`, return _the length of a maximum size turbulent subarray of_ `arr`.

A subarray is **turbulent** if the comparison sign flips between each adjacent pair of elements in the subarray.

More formally, a subarray `[arr[i], arr[i + 1], ..., arr[j]]` of `arr` is said to be turbulent if and only if:

*   For `i <= k < j`:
    *   `arr[k] > arr[k + 1]` when `k` is odd, and
    *   `arr[k] < arr[k + 1]` when `k` is even.
*   Or, for `i <= k < j`:
    *   `arr[k] > arr[k + 1]` when `k` is even, and
    *   `arr[k] < arr[k + 1]` when `k` is odd.

**Example 1:**

**Input:** arr = [9,4,2,10,7,8,8,1,9]

**Output:** 5

**Explanation:** arr[1] > arr[2] < arr[3] > arr[4] < arr[5]

**Example 2:**

**Input:** arr = [4,8,12,16]

**Output:** 2

**Example 3:**

**Input:** arr = [100]

**Output:** 1

**Constraints:**

*   <code>1 <= arr.length <= 4 * 10<sup>4</sup></code>
*   <code>0 <= arr[i] <= 10<sup>9</sup></code>

## Solution

```kotlin
class Solution {
    fun maxTurbulenceSize(arr: IntArray): Int {
        val n = arr.size
        var ans = 1
        var l: Int
        if (n == 1) {
            return 1
        }
        if (n == 2) {
            return if (arr[0] == arr[1]) 1 else 2
        }
        l = 0
        var r = 1
        while (r < n - 1) {
            val difL = arr[r] - arr[r - 1]
            val difR = arr[r] - arr[r + 1]
            if (difL == 0 && difR == 0) {
                l = r + 1
            } else if (difL == 0) {
                ans = ans.coerceAtLeast(r - l)
                l = r
            } else if (!(difL < 0 && difR < 0 || difL > 0 && difR > 0)) {
                ans = ans.coerceAtLeast(r - l + 1)
                l = r
            }
            r++
        }
        return ans.coerceAtLeast(r - l + 1)
    }
}
```