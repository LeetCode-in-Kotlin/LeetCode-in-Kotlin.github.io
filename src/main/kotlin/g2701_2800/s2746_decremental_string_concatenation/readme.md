[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2746\. Decremental String Concatenation

Medium

You are given a **0-indexed** array `words` containing `n` strings.

Let's define a **join** operation `join(x, y)` between two strings `x` and `y` as concatenating them into `xy`. However, if the last character of `x` is equal to the first character of `y`, one of them is **deleted**.

For example `join("ab", "ba") = "aba"` and `join("ab", "cde") = "abcde"`.

You are to perform `n - 1` **join** operations. Let <code>str<sub>0</sub> = words[0]</code>. Starting from `i = 1` up to `i = n - 1`, for the <code>i<sup>th</sup></code> operation, you can do one of the following:

*   Make <code>str<sub>i</sub> = join(str<sub>i - 1</sub>, words[i])</code>
*   Make <code>str<sub>i</sub> = join(words[i], str<sub>i - 1</sub>)</code>

Your task is to **minimize** the length of <code>str<sub>n - 1</sub></code>.

Return _an integer denoting the minimum possible length of_ <code>str<sub>n - 1</sub></code>.

**Example 1:**

**Input:** words = ["aa","ab","bc"]

**Output:** 4

**Explanation:** In this example, we can perform join operations in the following order to minimize the length of str<sub>2</sub>: 

str<sub>0</sub> = "aa" 

str<sub>1</sub> = join(str<sub>0</sub>, "ab") = "aab" 

str<sub>2</sub> = join(str<sub>1</sub>, "bc") = "aabc" 

It can be shown that the minimum possible length of str<sub>2</sub> is 4.

**Example 2:**

**Input:** words = ["ab","b"]

**Output:** 2

**Explanation:** In this example, str<sub>0</sub> = "ab", there are two ways to get str<sub>1</sub>: join(str<sub>0</sub>, "b") = "ab" or join("b", str<sub>0</sub>) = "bab". The first string, "ab", has the minimum length. Hence, the answer is 2.

**Example 3:**

**Input:** words = ["aaa","c","aba"]

**Output:** 6

**Explanation:** In this example, we can perform join operations in the following order to minimize the length of str<sub>2</sub>: 

str<sub>0</sub> = "aaa" 

str<sub>1</sub> = join(str<sub>0</sub>, "c") = "aaac" 

str<sub>2</sub> = join("aba", str<sub>1</sub>) = "abaaac" 

It can be shown that the minimum possible length of str<sub>2</sub> is 6.

**Constraints:**

*   `1 <= words.length <= 1000`
*   `1 <= words[i].length <= 50`
*   Each character in `words[i]` is an English lowercase letter

## Solution

```kotlin
class Solution {
    private val inf = 1e9.toInt()
    private lateinit var dp: Array<Array<Array<Int?>>>

    fun minimizeConcatenatedLength(words: Array<String>): Int {
        val n = words.size
        dp = Array(n) {
            Array(26) {
                arrayOfNulls(
                    26,
                )
            }
        }
        val curWord = words[0]
        val curLen = curWord.length
        val curFirst = curWord[0]
        val curLast = curWord[curLen - 1]
        return curLen + solve(1, curFirst, curLast, n, words)
    }

    private fun solve(idx: Int, prevFirst: Char, prevLast: Char, n: Int, words: Array<String>): Int {
        if (idx == n) return 0
        if (dp[idx][prevFirst.code - 'a'.code][prevLast.code - 'a'.code] != null) {
            return dp[idx][prevFirst.code - 'a'.code][prevLast.code - 'a'.code]!!
        }
        val curWord = words[idx]
        val curLen = curWord.length
        val curFirst = curWord[0]
        val curLast = curWord[curLen - 1]
        var ans = inf
        ans = if (prevFirst == curLast) {
            ans.coerceAtMost(curLen - 1 + solve(idx + 1, curFirst, prevLast, n, words))
        } else {
            ans.coerceAtMost(curLen + solve(idx + 1, curFirst, prevLast, n, words))
        }
        ans = if (prevLast == curFirst) {
            ans.coerceAtMost(curLen - 1 + solve(idx + 1, prevFirst, curLast, n, words))
        } else {
            ans.coerceAtMost(curLen + solve(idx + 1, prevFirst, curLast, n, words))
        }
        return ans.also { dp[idx][prevFirst.code - 'a'.code][prevLast.code - 'a'.code] = it }
    }
}
```