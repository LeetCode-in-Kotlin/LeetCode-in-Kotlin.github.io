[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 388\. Longest Absolute File Path

Medium

Suppose we have a file system that stores both files and directories. An example of one system is represented in the following picture:

![](https://assets.leetcode.com/uploads/2020/08/28/mdir.jpg)

Here, we have `dir` as the only directory in the root. `dir` contains two subdirectories, `subdir1` and `subdir2`. `subdir1` contains a file `file1.ext` and subdirectory `subsubdir1`. `subdir2` contains a subdirectory `subsubdir2`, which contains a file `file2.ext`.

In text form, it looks like this (with ⟶ representing the tab character):

    dir 
    ⟶ subdir1
    ⟶ ⟶ file1.ext
    ⟶ ⟶ subsubdir1
    ⟶ subdir2
    ⟶ ⟶ subsubdir2
    ⟶ ⟶ ⟶ file2.ext

If we were to write this representation in code, it will look like this: 
`"dir\tsubdir1\t\tfile1.ext\t\tsubsubdir1\tsubdir2\t\tsubsubdir2\t\t\tfile2.ext"`. Note that the `'  
'` and `'\t'` are the new-line and tab characters.

Every file and directory has a unique **absolute path** in the file system, which is the order of directories that must be opened to reach the file/directory itself, all concatenated by `'/'s`. Using the above example, the **absolute path** to `file2.ext` is `"dir/subdir2/subsubdir2/file2.ext"`. Each directory name consists of letters, digits, and/or spaces. Each file name is of the form `name.extension`, where `name` and `extension` consist of letters, digits, and/or spaces.

Given a string `input` representing the file system in the explained format, return _the length of the **longest absolute path** to a **file** in the abstracted file system_. If there is no file in the system, return `0`.

**Note** that the testcases are generated such that the file system is valid and no file or directory name has length 0.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/08/28/dir1.jpg)

**Input:** input = "dir\n\\tsubdir1\n\\tsubdir2\n\\t\\tfile.ext"

**Output:** 20

**Explanation:** We have only one file, and the absolute path is "dir/subdir2/file.ext" of length 20. 

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/08/28/dir2.jpg)

**Input:** input = "dir\n\\tsubdir1\n\\t\\tfile1.ext\n\\t\\tsubsubdir1\n\\tsubdir2\n\\t\\tsubsubdir2\n\\t\\t\\tfile2.ext"

**Output:** 32

**Explanation:** We have two files:

"dir/subdir1/file1.ext" of length 21

"dir/subdir2/subsubdir2/file2.ext" of length 32.

We return 32 since it is the longest absolute path to a file. 

**Example 3:**

**Input:** input = "a"

**Output:** 0

**Explanation:** We do not have any files, just a single directory named "a". 

**Constraints:**

*   <code>1 <= input.length <= 10<sup>4</sup></code>
*   `input` may contain lowercase or uppercase English letters, a new line character `'  
    '`, a tab character `'\t'`, a dot `'.'`, a space `' '`, and digits.
*   All file and directory names have **positive** length.

## Solution

```kotlin
import java.util.ArrayDeque
import java.util.Deque

class Solution {
    fun lengthLongestPath(input: String): Int {
        val stack: Deque<Int> = ArrayDeque()
        var longestLen = 0
        var currDirLen = 0
        var i = 0
        var currLevel: Int
        var nextLevel = 0
        var isFile = false
        val period = '.'
        val space = ' '
        while (i < input.length) {
            currLevel = nextLevel
            var currStrLen = 0
            while (i < input.length &&
                (Character.isLetterOrDigit(input[i]) || period == input[i] || space == input[i])
            ) {
                if (period == input[i]) {
                    isFile = true
                }
                i++
                currStrLen++
            }
            if (isFile) {
                longestLen = Math.max(longestLen, currDirLen + currStrLen)
            } else {
                currDirLen += currStrLen + 1
                stack.push(currStrLen + 1)
            }
            nextLevel = 0
            // increment one to let it pass "\n" and start from "\t"
            i = i + 1
            while (i < input.length - 1 && input[i] == '\t') {
                nextLevel++
                i = i + 1
            }
            if (nextLevel < currLevel) {
                var j = 0
                if (isFile) {
                    while (stack.isNotEmpty() && j < currLevel - nextLevel) {
                        currDirLen -= stack.pop()
                        j++
                    }
                } else {
                    while (stack.isNotEmpty() && j <= currLevel - nextLevel) {
                        currDirLen -= stack.pop()
                        j++
                    }
                }
            } else if (nextLevel == currLevel && !isFile && stack.isNotEmpty()) {
                currDirLen -= stack.pop()
            }
            if (nextLevel == 0) {
                currDirLen = 0
                stack.clear()
            }
            isFile = false
        }
        return longestLen
    }
}
```