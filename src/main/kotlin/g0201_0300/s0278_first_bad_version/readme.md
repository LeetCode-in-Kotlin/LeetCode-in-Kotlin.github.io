[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 278\. First Bad Version

Easy

You are a product manager and currently leading a team to develop a new product. Unfortunately, the latest version of your product fails the quality check. Since each version is developed based on the previous version, all the versions after a bad version are also bad.

Suppose you have `n` versions `[1, 2, ..., n]` and you want to find out the first bad one, which causes all the following ones to be bad.

You are given an API `bool isBadVersion(version)` which returns whether `version` is bad. Implement a function to find the first bad version. You should minimize the number of calls to the API.

**Example 1:**

**Input:** n = 5, bad = 4

**Output:** 4

**Explanation:**

    call isBadVersion(3) -> false
    call isBadVersion(5) -> true
    call isBadVersion(4) -> true
    Then 4 is the first bad version. 

**Example 2:**

**Input:** n = 1, bad = 1

**Output:** 1

**Constraints:**

*   <code>1 <= bad <= n <= 2<sup>31</sup> - 1</code>

## Solution

```kotlin
/* The isBadVersion API is defined in the parent class VersionControl.
      fun isBadVersion(version: Int) : Boolean {} */

class Solution : VersionControl() {
    fun firstBadVersion(n: Int): Int {
        var l = 1
        var r = n
        var mid: Int
        while (l < r) {
            mid = l + (r - l) / 2
            if (!isBadVersion(mid)) {
                l = mid + 1
            } else {
                r = mid
            }
        }
        return l
    }
}
```