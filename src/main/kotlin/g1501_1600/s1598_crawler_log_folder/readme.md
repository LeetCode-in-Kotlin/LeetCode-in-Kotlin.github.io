[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1598\. Crawler Log Folder

Easy

The Leetcode file system keeps a log each time some user performs a _change folder_ operation.

The operations are described below:

*   `"../"` : Move to the parent folder of the current folder. (If you are already in the main folder, **remain in the same folder**).
*   `"./"` : Remain in the same folder.
*   `"x/"` : Move to the child folder named `x` (This folder is **guaranteed to always exist**).

You are given a list of strings `logs` where `logs[i]` is the operation performed by the user at the <code>i<sup>th</sup></code> step.

The file system starts in the main folder, then the operations in `logs` are performed.

Return _the minimum number of operations needed to go back to the main folder after the change folder operations._

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/09/09/sample_11_1957.png)

**Input:** logs = ["d1/","d2/","../","d21/","./"]

**Output:** 2

**Explanation:** Use this change folder operation "../" 2 times and go back to the main folder.

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/09/09/sample_22_1957.png)

**Input:** logs = ["d1/","d2/","./","d3/","../","d31/"]

**Output:** 3

**Example 3:**

**Input:** logs = ["d1/","../","../","../"]

**Output:** 0

**Constraints:**

*   <code>1 <= logs.length <= 10<sup>3</sup></code>
*   `2 <= logs[i].length <= 10`
*   `logs[i]` contains lowercase English letters, digits, `'.'`, and `'/'`.
*   `logs[i]` follows the format described in the statement.
*   Folder names consist of lowercase English letters and digits.

## Solution

```kotlin
class Solution {
    fun minOperations(logs: Array<String>): Int {
        var steps = 0
        for (log in logs) {
            if (log == "../") {
                if (steps > 0) {
                    steps--
                }
            } else if (log != "./") {
                steps++
            }
        }
        return steps
    }
}
```