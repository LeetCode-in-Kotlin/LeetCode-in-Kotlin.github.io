[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1125\. Smallest Sufficient Team

Hard

In a project, you have a list of required skills `req_skills`, and a list of people. The <code>i<sup>th</sup></code> person `people[i]` contains a list of skills that the person has.

Consider a sufficient team: a set of people such that for every required skill in `req_skills`, there is at least one person in the team who has that skill. We can represent these teams by the index of each person.

*   For example, `team = [0, 1, 3]` represents the people with skills `people[0]`, `people[1]`, and `people[3]`.

Return _any sufficient team of the smallest possible size, represented by the index of each person_. You may return the answer in **any order**.

It is **guaranteed** an answer exists.

**Example 1:**

**Input:** req\_skills = ["java","nodejs","reactjs"], people = \[\["java"],["nodejs"],["nodejs","reactjs"]]

**Output:** [0,2]

**Example 2:**

**Input:** req\_skills = ["algorithms","math","java","reactjs","csharp","aws"], people = \[\["algorithms","math","java"],["algorithms","math","reactjs"],["java","csharp","aws"],["reactjs","csharp"],["csharp","math"],["aws","java"]]

**Output:** [1,2]

**Constraints:**

*   `1 <= req_skills.length <= 16`
*   `1 <= req_skills[i].length <= 16`
*   `req_skills[i]` consists of lowercase English letters.
*   All the strings of `req_skills` are **unique**.
*   `1 <= people.length <= 60`
*   `0 <= people[i].length <= 16`
*   `1 <= people[i][j].length <= 16`
*   `people[i][j]` consists of lowercase English letters.
*   All the strings of `people[i]` are **unique**.
*   Every skill in `people[i]` is a skill in `req_skills`.
*   It is guaranteed a sufficient team exists.

## Solution

```kotlin
class Solution {
    private var ans: List<Int> = ArrayList()
    fun smallestSufficientTeam(skills: Array<String>, people: List<List<String>>): IntArray {
        val n = skills.size
        val m = people.size
        val map: MutableMap<String, Int> = HashMap()
        for (i in 0 until n) {
            map[skills[i]] = i
        }
        val arr = IntArray(m)
        for (i in 0 until m) {
            val list = people[i]
            var `val` = 0
            for (skill in list) {
                `val` = `val` or (1 shl map[skill]!!)
            }
            arr[i] = `val`
        }
        val banned = BooleanArray(m)
        for (i in 0 until m) {
            for (j in i + 1 until m) {
                val `val` = arr[i] or arr[j]
                if (`val` == arr[i]) {
                    banned[j] = true
                } else if (`val` == arr[j]) {
                    banned[i] = true
                }
            }
        }
        helper(0, n, arr, mutableListOf(), banned)
        val res = IntArray(ans.size)
        for (i in res.indices) {
            res[i] = ans[i]
        }
        return res
    }

    private fun helper(cur: Int, n: Int, arr: IntArray, list: MutableList<Int>, banned: BooleanArray) {
        if (cur == (1 shl n) - 1) {
            if (ans.isEmpty() || ans.size > list.size) {
                ans = ArrayList(list)
            }
            return
        }
        if (ans.isNotEmpty() && list.size >= ans.size) {
            return
        }
        var zero = 0
        while (cur shr zero and 1 == 1) {
            zero++
        }
        for (i in arr.indices) {
            if (banned[i]) {
                continue
            }
            if (arr[i] shr zero and 1 == 1) {
                list.add(i)
                helper(cur or arr[i], n, arr, list, banned)
                list.removeAt(list.size - 1)
            }
        }
    }
}
```