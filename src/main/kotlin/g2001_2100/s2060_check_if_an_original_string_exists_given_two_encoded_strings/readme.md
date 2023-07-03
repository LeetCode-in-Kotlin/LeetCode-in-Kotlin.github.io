[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2060\. Check if an Original String Exists Given Two Encoded Strings

Hard

An original string, consisting of lowercase English letters, can be encoded by the following steps:

*   Arbitrarily **split** it into a **sequence** of some number of **non-empty** substrings.
*   Arbitrarily choose some elements (possibly none) of the sequence, and **replace** each with **its length** (as a numeric string).
*   **Concatenate** the sequence as the encoded string.

For example, **one way** to encode an original string `"abcdefghijklmnop"` might be:

*   Split it as a sequence: `["ab", "cdefghijklmn", "o", "p"]`.
*   Choose the second and third elements to be replaced by their lengths, respectively. The sequence becomes `["ab", "12", "1", "p"]`.
*   Concatenate the elements of the sequence to get the encoded string: `"ab121p"`.

Given two encoded strings `s1` and `s2`, consisting of lowercase English letters and digits `1-9` (inclusive), return `true` _if there exists an original string that could be encoded as **both**_ `s1` _and_ `s2`_. Otherwise, return_ `false`.

**Note**: The test cases are generated such that the number of consecutive digits in `s1` and `s2` does not exceed `3`.

**Example 1:**

**Input:** s1 = "internationalization", s2 = "i18n"

**Output:** true

**Explanation:** It is possible that "internationalization" was the original string. 

- "internationalization" 
  
    -> Split: ["internationalization"] 
  
    -> Do not replace any element 
    
    -> Concatenate: "internationalization", which is s1. 
    
- "internationalization" 
  
    -> Split: ["i", "nternationalizatio", "n"]
  
    -> Replace: ["i", "18", "n"] 
  
    -> Concatenate: "i18n", which is s2

**Example 2:**

**Input:** s1 = "l123e", s2 = "44"

**Output:** true

**Explanation:** It is possible that "leetcode" was the original string. 

- "leetcode" 
  
    -> Split: ["l", "e", "et", "cod", "e"] 
  
    -> Replace: ["l", "1", "2", "3", "e"] 
  
    -> Concatenate: "l123e", which is s1. 
    
- "leetcode" 
  
    -> Split: ["leet", "code"] 
  
    -> Replace: ["4", "4"] 
  
    -> Concatenate: "44", which is s2.

**Example 3:**

**Input:** s1 = "a5b", s2 = "c5b"

**Output:** false

**Explanation:** It is impossible. 

- The original string encoded as s1 must start with the letter 'a'. 

- The original string encoded as s2 must start with the letter 'c'.

**Constraints:**

*   `1 <= s1.length, s2.length <= 40`
*   `s1` and `s2` consist of digits `1-9` (inclusive), and lowercase English letters only.
*   The number of consecutive digits in `s1` and `s2` does not exceed `3`.

## Solution

```kotlin
class Solution {
    private var stringMatched = false
    private var s1: String? = null
    private var s2: String? = null
    private lateinit var memo: Array<Array<Array<Boolean?>>>

    fun possiblyEquals(s1: String, s2: String): Boolean {
        this.s1 = s1
        this.s2 = s2
        memo = Array(s1.length + 1) { Array<Array<Boolean?>>(s2.length + 1) { arrayOfNulls(2000) } }
        dfs(0, 0, 0)
        return stringMatched
    }

    private fun dfs(i1: Int, i2: Int, diff: Int) {
        if (stringMatched) {
            return
        }
        if (i1 == s1!!.length && i2 == s2!!.length) {
            if (diff == 0) {
                stringMatched = true
            }
            return
        }
        if (i1 == s1!!.length && diff <= 0) {
            return
        }
        if (i2 == s2!!.length && diff >= 0) {
            return
        }
        if (memo[i1][i2][diff + 999] != null) {
            stringMatched = memo[i1][i2][diff + 999]!!
            return
        }
        val indexNums1: MutableList<IntArray> = ArrayList()
        var num1 = 0
        var x1 = i1
        while (x1 < s1!!.length && Character.isDigit(s1!![x1])) {
            num1 = num1 * 10 + (s1!![x1].code - '0'.code)
            indexNums1.add(intArrayOf(x1, num1))
            x1++
        }
        val indexNums2: MutableList<IntArray> = ArrayList()
        var num2 = 0
        var x2 = i2
        while (x2 < s2!!.length && Character.isDigit(s2!![x2])) {
            num2 = num2 * 10 + (s2!![x2].code - '0'.code)
            indexNums2.add(intArrayOf(x2, num2))
            x2++
        }
        if (diff == 0) {
            if (extracted(i1, i2, diff, indexNums1, indexNums2)) {
                return
            }
        } else if (diff > 0) {
            if (Character.isLetter(s2!![i2])) {
                dfs(i1, i2 + 1, diff - 1)
            } else {
                for (num2Item in indexNums2) {
                    dfs(i1, num2Item[0] + 1, diff - num2Item[1])
                }
            }
        } else {
            if (Character.isLetter(s1!![i1])) {
                dfs(i1 + 1, i2, diff + 1)
            } else {
                for (num1Item in indexNums1) {
                    dfs(num1Item[0] + 1, i2, diff + num1Item[1])
                }
            }
        }
        memo[i1][i2][diff + 999] = stringMatched
    }

    private fun extracted(
        i1: Int,
        i2: Int,
        diff: Int,
        indexNums1: List<IntArray>,
        indexNums2: List<IntArray>
    ): Boolean {
        val c1 = s1!![i1]
        val c2 = s2!![i2]
        if (Character.isLetter(c1) && Character.isLetter(c2)) {
            if (c1 != c2) {
                return true
            }
            dfs(i1 + 1, i2 + 1, diff)
            return true
        } else {
            if (indexNums1.isNotEmpty()) {
                for (num1Item in indexNums1) {
                    dfs(num1Item[0] + 1, i2, diff + num1Item[1])
                }
            } else {
                for (num2Item in indexNums2) {
                    dfs(i1, num2Item[0] + 1, diff - num2Item[1])
                }
            }
        }
        return false
    }
}
```