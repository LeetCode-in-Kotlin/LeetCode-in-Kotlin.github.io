[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2213\. Longest Substring of One Repeating Character

Hard

You are given a **0-indexed** string `s`. You are also given a **0-indexed** string `queryCharacters` of length `k` and a **0-indexed** array of integer **indices** `queryIndices` of length `k`, both of which are used to describe `k` queries.

The <code>i<sup>th</sup></code> query updates the character in `s` at index `queryIndices[i]` to the character `queryCharacters[i]`.

Return _an array_ `lengths` _of length_ `k` _where_ `lengths[i]` _is the **length** of the **longest substring** of_ `s` _consisting of **only one repeating** character **after** the_ <code>i<sup>th</sup></code> _query_ _is performed._

**Example 1:**

**Input:** s = "babacc", queryCharacters = "bcb", queryIndices = [1,3,3]

**Output:** [3,3,4]

**Explanation:** - 1<sup>st</sup> query updates s = "b**b**bacc". The longest substring consisting of one repeating character is "bbb" with length 3. 

- 2<sup>nd</sup> query updates s = "bbb**c**cc". The longest substring consisting of one repeating character can be "bbb" or "ccc" with length 3. 

- 3<sup>rd</sup> query updates s = "bbb**b**cc". The longest substring consisting of one repeating character is "bbbb" with length 4. 
  
Thus, we return [3,3,4].

**Example 2:**

**Input:** s = "abyzz", queryCharacters = "aa", queryIndices = [2,1]

**Output:** [2,3]

**Explanation:** - 1<sup>st</sup> query updates s = "ab**a**zz". The longest substring consisting of one repeating character is "zz" with length 2. 

- 2<sup>nd</sup> query updates s = "a**a**azz". The longest substring consisting of one repeating character is "aaa" with length 3. 
  
Thus, we return [2,3].

**Constraints:**

*   <code>1 <= s.length <= 10<sup>5</sup></code>
*   `s` consists of lowercase English letters.
*   `k == queryCharacters.length == queryIndices.length`
*   <code>1 <= k <= 10<sup>5</sup></code>
*   `queryCharacters` consists of lowercase English letters.
*   `0 <= queryIndices[i] < s.length`

## Solution

```kotlin
class Solution {
    internal class TreeNode(var start: Int, var end: Int) {
        var leftChar = 0.toChar()
        var leftCharLen = 0
        var rightChar = 0.toChar()
        var rightCharLen = 0
        var max = 0
        var left: TreeNode? = null
        var right: TreeNode? = null
    }

    fun longestRepeating(s: String, queryCharacters: String, queryIndices: IntArray): IntArray {
        val sChar = s.toCharArray()
        val qChar = queryCharacters.toCharArray()
        val root = buildTree(sChar, 0, sChar.size - 1)
        val result = IntArray(qChar.size)
        for (i in qChar.indices) {
            updateTree(root, queryIndices[i], qChar[i])
            if (root != null) {
                result[i] = root.max
            }
        }
        return result
    }

    private fun buildTree(s: CharArray, from: Int, to: Int): TreeNode? {
        if (from > to) {
            return null
        }
        val root = TreeNode(from, to)
        if (from == to) {
            root.max = 1
            root.leftChar = s[from]
            root.rightChar = root.leftChar
            root.rightCharLen = 1
            root.leftCharLen = root.rightCharLen
            return root
        }
        val middle = from + (to - from) / 2
        root.left = buildTree(s, from, middle)
        root.right = buildTree(s, middle + 1, to)
        updateNode(root)
        return root
    }

    private fun updateTree(root: TreeNode?, index: Int, c: Char) {
        if (root == null || root.start > index || root.end < index) {
            return
        }
        if (root.start == index && root.end == index) {
            root.rightChar = c
            root.leftChar = root.rightChar
            return
        }
        updateTree(root.left, index, c)
        updateTree(root.right, index, c)
        updateNode(root)
    }

    private fun updateNode(root: TreeNode?) {
        if (root == null) {
            return
        }
        root.leftChar = root.left!!.leftChar
        root.leftCharLen = root.left!!.leftCharLen
        root.rightChar = root.right!!.rightChar
        root.rightCharLen = root.right!!.rightCharLen
        root.max = Math.max(root.left!!.max, root.right!!.max)
        if (root.left!!.rightChar == root.right!!.leftChar) {
            val len = root.left!!.rightCharLen + root.right!!.leftCharLen
            if (root.left!!.leftChar == root.left!!.rightChar &&
                root.left!!.leftCharLen == root.left!!.end - root.left!!.start + 1
            ) {
                root.leftCharLen = len
            }
            if (root.right!!.leftChar == root.right!!.rightChar &&
                root.right!!.leftCharLen == root.right!!.end - root.right!!.start + 1
            ) {
                root.rightCharLen = len
            }
            root.max = Math.max(root.max, len)
        }
    }
}
```