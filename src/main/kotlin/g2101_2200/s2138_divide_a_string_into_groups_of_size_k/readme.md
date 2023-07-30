[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2138\. Divide a String Into Groups of Size k

Easy

A string `s` can be partitioned into groups of size `k` using the following procedure:

*   The first group consists of the first `k` characters of the string, the second group consists of the next `k` characters of the string, and so on. Each character can be a part of **exactly one** group.
*   For the last group, if the string **does not** have `k` characters remaining, a character `fill` is used to complete the group.

Note that the partition is done so that after removing the `fill` character from the last group (if it exists) and concatenating all the groups in order, the resultant string should be `s`.

Given the string `s`, the size of each group `k` and the character `fill`, return _a string array denoting the **composition of every group**_ `s` _has been divided into, using the above procedure_.

**Example 1:**

**Input:** s = "abcdefghi", k = 3, fill = "x"

**Output:** ["abc","def","ghi"]

**Explanation:** 

The first 3 characters "abc" form the first group. 

The next 3 characters "def" form the second group. 

The last 3 characters "ghi" form the third group. 

Since all groups can be completely filled by characters from the string, we do not need to use fill. 

Thus, the groups formed are "abc", "def", and "ghi".

**Example 2:**

**Input:** s = "abcdefghij", k = 3, fill = "x"

**Output:** ["abc","def","ghi","jxx"]

**Explanation:** 

Similar to the previous example, we are forming the first three groups "abc", "def", and "ghi". 

For the last group, we can only use the character 'j' from the string. 

To complete this group, we add 'x' twice. Thus, the 4 groups formed are "abc", "def", "ghi", and "jxx".

**Constraints:**

*   `1 <= s.length <= 100`
*   `s` consists of lowercase English letters only.
*   `1 <= k <= 100`
*   `fill` is a lowercase English letter.

## Solution

```kotlin
class Solution {
    fun divideString(s: String, k: Int, fill: Char): Array<String?> {
        val ans = arrayOfNulls<String>(if (s.length % k != 0) s.length / k + 1 else s.length / k)
        var t = k
        val str: MutableList<String> = ArrayList()
        val sb = StringBuilder()
        var i = 0
        while (i < s.length) {
            if (t > 0) {
                sb.append(s[i])
                t--
            } else {
                t = k
                str.add(sb.toString())
                sb.setLength(0)
                i--
            }
            i++
        }
        if (t > 0) {
            while (t-- > 0) {
                sb.append(fill)
            }
        }
        str.add(sb.toString())
        for (j in str.indices) {
            ans[j] = str[j]
        }
        return ans
    }
}
```