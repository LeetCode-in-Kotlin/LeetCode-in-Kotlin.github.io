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
    private lateinit var ca: CharArray

    fun longestRepeating(s: String, queryCharacters: String, queryIndices: IntArray): IntArray {
        ca = s.toCharArray()
        val result = IntArray(queryIndices.size)
        val root = SegmentTree(0, ca.size)
        for (i in queryIndices.indices) {
            ca[queryIndices[i]] = queryCharacters[i]
            root.update(queryIndices[i])
            result[i] = root.longest
        }
        return result
    }

    private inner class SegmentTree(val start: Int, val end: Int) {
        var longest: Int = 0
        var leftLength: Int = 0
        var rightLength: Int = 0
        private lateinit var left: SegmentTree
        private lateinit var right: SegmentTree

        init {
            if (end - start > 1) {
                val mid = (start + end) / 2
                left = SegmentTree(start, mid)
                right = SegmentTree(mid, end)
                merge()
            } else {
                longest = 1
                leftLength = 1
                rightLength = 1
            }
        }

        fun update(index: Int) {
            if (end - start == 1) return
            if (index < (left.end)) {
                left.update(index)
            } else {
                right.update(index)
            }
            merge()
        }

        private fun merge() {
            longest = maxOf(left.longest, right.longest)
            if (ca[left.end - 1] == ca[right.start]) {
                longest = maxOf(longest, left.rightLength + right.leftLength)
                leftLength = if (left.leftLength == left.end - left.start) {
                    left.leftLength + right.leftLength
                } else {
                    left.leftLength
                }
                rightLength = if (right.rightLength == (right.end - right.start)) {
                    right.rightLength + left.rightLength
                } else {
                    right.rightLength
                }
            } else {
                leftLength = left.leftLength
                rightLength = right.rightLength
            }
        }
    }
}
```