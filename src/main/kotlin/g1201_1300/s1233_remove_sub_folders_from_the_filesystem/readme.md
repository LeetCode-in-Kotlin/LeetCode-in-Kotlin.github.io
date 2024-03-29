[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1233\. Remove Sub-Folders from the Filesystem

Medium

Given a list of folders `folder`, return _the folders after removing all **sub-folders** in those folders_. You may return the answer in **any order**.

If a `folder[i]` is located within another `folder[j]`, it is called a **sub-folder** of it.

The format of a path is one or more concatenated strings of the form: `'/'` followed by one or more lowercase English letters.

*   For example, `"/leetcode"` and `"/leetcode/problems"` are valid paths while an empty string and `"/"` are not.

**Example 1:**

**Input:** folder = ["/a","/a/b","/c/d","/c/d/e","/c/f"]

**Output:** ["/a","/c/d","/c/f"]

**Explanation:** Folders "/a/b" is a subfolder of "/a" and "/c/d/e" is inside of folder "/c/d" in our filesystem.

**Example 2:**

**Input:** folder = ["/a","/a/b/c","/a/b/d"]

**Output:** ["/a"]

**Explanation:** Folders "/a/b/c" and "/a/b/d" will be removed because they are subfolders of "/a".

**Example 3:**

**Input:** folder = ["/a/b/c","/a/b/ca","/a/b/d"]

**Output:** ["/a/b/c","/a/b/ca","/a/b/d"]

**Constraints:**

*   <code>1 <= folder.length <= 4 * 10<sup>4</sup></code>
*   `2 <= folder[i].length <= 100`
*   `folder[i]` contains only lowercase letters and `'/'`.
*   `folder[i]` always starts with the character `'/'`.
*   Each folder name is **unique**.

## Solution

```kotlin
class Solution {
    fun removeSubfolders(folder: Array<String>): List<String> {
        val paths: MutableSet<String> = HashSet()
        paths.addAll(folder)
        val res: MutableList<String> = ArrayList()
        for (f in folder) {
            var lastSlash = f.lastIndexOf("/")
            var isSub = false
            while (lastSlash > 0) {
                val upperDir = f.substring(0, lastSlash)
                if (paths.contains(upperDir)) {
                    isSub = true
                    break
                }
                lastSlash = upperDir.lastIndexOf("/")
            }
            if (!isSub) {
                res.add(f)
            }
        }
        return res
    }
}
```