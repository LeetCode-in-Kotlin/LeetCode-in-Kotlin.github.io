[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 17\. Letter Combinations of a Phone Number

Medium

Given a string containing digits from `2-9` inclusive, return all possible letter combinations that the number could represent. Return the answer in **any order**.

A mapping of digit to letters (just like on the telephone buttons) is given below. Note that 1 does not map to any letters.

![](https://upload.wikimedia.org/wikipedia/commons/thumb/7/73/Telephone-keypad2.svg/200px-Telephone-keypad2.svg.png)

**Example 1:**

**Input:** digits = "23"

**Output:** ["ad","ae","af","bd","be","bf","cd","ce","cf"]

**Example 2:**

**Input:** digits = ""

**Output:** []

**Example 3:**

**Input:** digits = "2"

**Output:** ["a","b","c"]

**Constraints:**

*   `0 <= digits.length <= 4`
*   `digits[i]` is a digit in the range `['2', '9']`.

## Solution

```kotlin
class Solution {
    fun letterCombinations(digits: String): List<String> {
        if (digits.isEmpty()) return ArrayList()
        val letters = arrayOf("", "", "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz")
        val ans: MutableList<String> = ArrayList()
        val sb = StringBuilder()
        findCombinations(0, digits, letters, sb, ans)
        return ans
    }

    private fun findCombinations(
        start: Int,
        nums: String,
        letters: Array<String>,
        curr: StringBuilder,
        ans: MutableList<String>,
    ) {
        if (curr.length == nums.length) {
            ans.add(curr.toString())
            return
        }
        for (i in start until nums.length) {
            val n = Character.getNumericValue(nums[i])
            for (j in 0 until letters[n].length) {
                val ch = letters[n][j]
                curr.append(ch)
                findCombinations(i + 1, nums, letters, curr, ans)
                curr.deleteCharAt(curr.length - 1)
            }
        }
    }
}
```