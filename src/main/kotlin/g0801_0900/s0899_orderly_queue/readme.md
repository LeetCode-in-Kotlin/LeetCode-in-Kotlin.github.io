[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 899\. Orderly Queue

Hard

You are given a string `s` and an integer `k`. You can choose one of the first `k` letters of `s` and append it at the end of the string..

Return _the lexicographically smallest string you could have after applying the mentioned step any number of moves_.

**Example 1:**

**Input:** s = "cba", k = 1

**Output:** "acb"

**Explanation:**

In the first move, we move the 1<sup>st</sup> character 'c' to the end, obtaining the string "bac".

In the second move, we move the 1<sup>st</sup> character 'b' to the end, obtaining the final result "acb".

**Example 2:**

**Input:** s = "baaca", k = 3

**Output:** "aaabc"

**Explanation:**

In the first move, we move the 1<sup>st</sup> character 'b' to the end, obtaining the string "aacab".

In the second move, we move the 3<sup>rd</sup> character 'c' to the end, obtaining the final result "aaabc".

**Constraints:**

*   `1 <= k <= s.length <= 1000`
*   `s` consist of lowercase English letters.

## Solution

```kotlin
class Solution {
    fun orderlyQueue(s: String, k: Int): String {
        if (k > 1) {
            val ans = s.toCharArray()
            ans.sort()
            return String(ans)
        }
        var min = 'z'
        val list = ArrayList<Int>()
        for (element in s) {
            if (element < min) {
                min = element
            }
        }
        for (i in s.indices) {
            if (s[i] == min) {
                list.add(i)
            }
        }
        var ans = s
        for (integer in list) {
            val after = s.substring(0, integer)
            val before = s.substring(integer)
            val f = before + after
            if (f < ans) {
                ans = f
            }
        }
        return ans
    }
}
```