[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 3527\. Find the Most Common Response

Medium

You are given a 2D string array `responses` where each `responses[i]` is an array of strings representing survey responses from the <code>i<sup>th</sup></code> day.

Return the **most common** response across all days after removing **duplicate** responses within each `responses[i]`. If there is a tie, return the _lexicographically smallest_ response.

**Example 1:**

**Input:** responses = \[\["good","ok","good","ok"],["ok","bad","good","ok","ok"],["good"],["bad"]]

**Output:** "good"

**Explanation:**

*   After removing duplicates within each list, `responses = \[\["good", "ok"], ["ok", "bad", "good"], ["good"], ["bad"]]`.
*   `"good"` appears 3 times, `"ok"` appears 2 times, and `"bad"` appears 2 times.
*   Return `"good"` because it has the highest frequency.

**Example 2:**

**Input:** responses = \[\["good","ok","good"],["ok","bad"],["bad","notsure"],["great","good"]]

**Output:** "bad"

**Explanation:**

*   After removing duplicates within each list we have `responses = \[\["good", "ok"], ["ok", "bad"], ["bad", "notsure"], ["great", "good"]]`.
*   `"bad"`, `"good"`, and `"ok"` each occur 2 times.
*   The output is `"bad"` because it is the lexicographically smallest amongst the words with the highest frequency.

**Constraints:**

*   `1 <= responses.length <= 1000`
*   `1 <= responses[i].length <= 1000`
*   `1 <= responses[i][j].length <= 10`
*   `responses[i][j]` consists of only lowercase English letters

## Solution

```kotlin
class Solution {
    private fun compareStrings(str1: String, str2: String): Boolean {
        val n = str1.length
        val m = str2.length
        var i = 0
        var j = 0
        while (i < n && j < m) {
            if (str1[i] < str2[j]) {
                return true
            } else if (str1[i] > str2[j]) {
                return false
            }
            i++
            j++
        }
        return n < m
    }

    fun findCommonResponse(responses: List<List<String>>): String {
        val n = responses.size
        val mp: MutableMap<String, IntArray> = HashMap()
        var ans = responses[0][0]
        var maxFreq = 0
        for (row in 0..<n) {
            val m = responses[row].size
            for (col in 0..<m) {
                val resp = responses[row][col]
                val arr = mp.getOrDefault(resp, intArrayOf(0, -1))
                if (arr[1] != row) {
                    arr[0]++
                    arr[1] = row
                    mp.put(resp, arr)
                }
                if (arr[0] > maxFreq ||
                    (ans != resp) && arr[0] == maxFreq && compareStrings(resp, ans)
                ) {
                    ans = resp
                    maxFreq = arr[0]
                }
            }
        }
        return ans
    }
}
```