[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 165\. Compare Version Numbers

Medium

Given two version numbers, `version1` and `version2`, compare them.

Version numbers consist of **one or more revisions** joined by a dot `'.'`. Each revision consists of **digits** and may contain leading **zeros**. Every revision contains **at least one character**. Revisions are **0-indexed from left to right**, with the leftmost revision being revision 0, the next revision being revision 1, and so on. For example `2.5.33` and `0.1` are valid version numbers.

To compare version numbers, compare their revisions in **left-to-right order**. Revisions are compared using their **integer value ignoring any leading zeros**. This means that revisions `1` and `001` are considered **equal**. If a version number does not specify a revision at an index, then **treat the revision as `0`**. For example, version `1.0` is less than version `1.1` because their revision 0s are the same, but their revision 1s are `0` and `1` respectively, and `0 < 1`.

_Return the following:_

*   If `version1 < version2`, return `-1`.
*   If `version1 > version2`, return `1`.
*   Otherwise, return `0`.

**Example 1:**

**Input:** version1 = "1.01", version2 = "1.001"

**Output:** 0

**Explanation:** Ignoring leading zeroes, both "01" and "001" represent the same integer "1".

**Example 2:**

**Input:** version1 = "1.0", version2 = "1.0.0"

**Output:** 0

**Explanation:** version1 does not specify revision 2, which means it is treated as "0".

**Example 3:**

**Input:** version1 = "0.1", version2 = "1.1"

**Output:** -1

**Explanation:** version1's revision 0 is "0", while version2's revision 0 is "1". 0 < 1, so version1 < version2.

**Constraints:**

*   `1 <= version1.length, version2.length <= 500`
*   `version1` and `version2` only contain digits and `'.'`.
*   `version1` and `version2` **are valid version numbers**.
*   All the given revisions in `version1` and `version2` can be stored in a **32-bit integer**.

## Solution

```kotlin
class Solution {
    fun compareVersion(version1: String, version2: String): Int {
        // acquire first number
        var numA = 0
        var i: Int
        i = 0
        while (i < version1.length) {
            val c = version1[i]
            numA = if (c == '.') {
                break
            } else {
                numA * 10 + (c.code - 48)
            }
            i++
        }
        // acquire second number
        var numB = 0
        var j: Int
        j = 0
        while (j < version2.length) {
            val c = version2[j]
            numB = if (c == '.') {
                break
            } else {
                numB * 10 + (c.code - 48)
            }
            j++
        }
        // compare
        return if (numA > numB) {
            1
        } else if (numA < numB) {
            -1
        } else {
            // equal
            var v1 = ""
            var v2 = ""
            if (i != version1.length) {
                v1 = version1.substring(i + 1)
            }
            if (j != version2.length) {
                v2 = version2.substring(j + 1)
            }
            // if both versions end here, they are equal
            if (v1 == "" && v2 == "") {
                0
            } else {
                compareVersion(v1, v2)
            }
        }
    }
}
```