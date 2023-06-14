[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1487\. Making File Names Unique

Medium

Given an array of strings `names` of size `n`. You will create `n` folders in your file system **such that**, at the <code>i<sup>th</sup></code> minute, you will create a folder with the name `names[i]`.

Since two files **cannot** have the same name, if you enter a folder name that was previously used, the system will have a suffix addition to its name in the form of `(k)`, where, `k` is the **smallest positive integer** such that the obtained name remains unique.

Return _an array of strings of length_ `n` where `ans[i]` is the actual name the system will assign to the <code>i<sup>th</sup></code> folder when you create it.

**Example 1:**

**Input:** names = ["pes","fifa","gta","pes(2019)"]

**Output:** ["pes","fifa","gta","pes(2019)"]

**Explanation:** Let's see how the file system creates folder names:

"pes" --> not assigned before, remains "pes"

"fifa" --> not assigned before, remains "fifa"

"gta" --> not assigned before, remains "gta"

"pes(2019)" --> not assigned before, remains "pes(2019)"

**Example 2:**

**Input:** names = ["gta","gta(1)","gta","avalon"]

**Output:** ["gta","gta(1)","gta(2)","avalon"]

**Explanation:** Let's see how the file system creates folder names:

"gta" --> not assigned before, remains "gta"

"gta(1)" --> not assigned before, remains "gta(1)"

"gta" --> the name is reserved, system adds (k), since "gta(1)" is also reserved, systems put k = 2. it becomes "gta(2)"

"avalon" --> not assigned before, remains "avalon"

**Example 3:**

**Input:** names = ["onepiece","onepiece(1)","onepiece(2)","onepiece(3)","onepiece"]

**Output:** ["onepiece","onepiece(1)","onepiece(2)","onepiece(3)","onepiece(4)"]

**Explanation:** When the last folder is created, the smallest positive valid k is 4, and it becomes "onepiece(4)".

**Constraints:**

*   <code>1 <= names.length <= 5 * 10<sup>4</sup></code>
*   `1 <= names[i].length <= 20`
*   `names[i]` consists of lowercase English letters, digits, and/or round brackets.

## Solution

```kotlin
class Solution {
    fun getFolderNames(names: Array<String>): Array<String> {
        val map = HashMap<String, Int>()
        for (i in names.indices) {
            var prefix = map.getOrDefault(names[i], 0)
            if (prefix != 0) {
                val raw = names[i]
                while (map.getOrDefault(names[i], 0) != 0) {
                    names[i] = "$raw($prefix)"
                    prefix++
                }
                map[raw] = prefix
            }
            map[names[i]] = 1
        }
        return names
    }
}
```