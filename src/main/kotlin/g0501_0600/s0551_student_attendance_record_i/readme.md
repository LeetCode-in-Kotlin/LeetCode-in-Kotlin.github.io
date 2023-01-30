[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 551\. Student Attendance Record I

Easy

You are given a string `s` representing an attendance record for a student where each character signifies whether the student was absent, late, or present on that day. The record only contains the following three characters:

*   `'A'`: Absent.
*   `'L'`: Late.
*   `'P'`: Present.

The student is eligible for an attendance award if they meet **both** of the following criteria:

*   The student was absent (`'A'`) for **strictly** fewer than 2 days **total**.
*   The student was **never** late (`'L'`) for 3 or more **consecutive** days.

Return `true` _if the student is eligible for an attendance award, or_ `false` _otherwise_.

**Example 1:**

**Input:** s = "PPALLP"

**Output:** true

**Explanation:** The student has fewer than 2 absences and was never late 3 or more consecutive days.

**Example 2:**

**Input:** s = "PPALLL"

**Output:** false

**Explanation:** The student was late 3 consecutive days in the last 3 days, so is not eligible for the award.

**Constraints:**

*   `1 <= s.length <= 1000`
*   `s[i]` is either `'A'`, `'L'`, or `'P'`.

## Solution

```kotlin
class Solution {
    fun checkRecord(s: String): Boolean {
        var aCount = 0
        var i = 0
        while (i < s.length) {
            if (s[i] == 'A') {
                aCount++
                if (aCount > 1) {
                    return false
                }
            } else if (s[i] == 'L') {
                var continuousLCount = 0
                while (i < s.length && s[i] == 'L') {
                    i++
                    continuousLCount++
                    if (continuousLCount > 2) {
                        return false
                    }
                }
                i--
            }
            i++
        }
        return true
    }
}
```