[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1733\. Minimum Number of People to Teach

Medium

On a social network consisting of `m` users and some friendships between users, two users can communicate with each other if they know a common language.

You are given an integer `n`, an array `languages`, and an array `friendships` where:

*   There are `n` languages numbered `1` through `n`,
*   `languages[i]` is the set of languages the <code>i<sup>th</sup></code> user knows, and
*   <code>friendships[i] = [u<sub>i</sub>, v<sub>i</sub>]</code> denotes a friendship between the users <code>u<sub>i</sub></code> and <code>v<sub>i</sub></code>.

You can choose **one** language and teach it to some users so that all friends can communicate with each other. Return _the_ _**minimum**_ _number of users you need to teach._

Note that friendships are not transitive, meaning if `x` is a friend of `y` and `y` is a friend of `z`, this doesn't guarantee that `x` is a friend of `z`.

**Example 1:**

**Input:** n = 2, languages = \[\[1],[2],[1,2]], friendships = \[\[1,2],[1,3],[2,3]]

**Output:** 1

**Explanation:** You can either teach user 1 the second language or user 2 the first language.

**Example 2:**

**Input:** n = 3, languages = \[\[2],[1,3],[1,2],[3]], friendships = \[\[1,4],[1,2],[3,4],[2,3]]

**Output:** 2

**Explanation:** Teach the third language to users 1 and 3, yielding two users to teach.

**Constraints:**

*   `2 <= n <= 500`
*   `languages.length == m`
*   `1 <= m <= 500`
*   `1 <= languages[i].length <= n`
*   `1 <= languages[i][j] <= n`
*   <code>1 <= u<sub>i</sub> < v<sub>i</sub> <= languages.length</code>
*   `1 <= friendships.length <= 500`
*   All tuples <code>(u<sub>i,</sub> v<sub>i</sub>)</code> are unique
*   `languages[i]` contains only unique values

## Solution

```kotlin
class Solution {
    fun minimumTeachings(n: Int, languages: Array<IntArray>, friendships: Array<IntArray>): Int {
        val m: Int = languages.size
        val speak: Array<BooleanArray> = Array(m + 1) { BooleanArray(n + 1) }
        val teach: Array<BooleanArray> = Array(m + 1) { BooleanArray(n + 1) }
        for (user in 0 until m) {
            val userLanguages: IntArray = languages[user]
            for (userLanguage: Int in userLanguages) {
                speak[user + 1][userLanguage] = true
            }
        }
        val listToTeach: MutableList<IntArray> = ArrayList()
        for (friend: IntArray in friendships) {
            val userA: Int = friend[0]
            val userB: Int = friend[1]
            var hasCommonLanguage = false
            for (language in 1..n) {
                if (speak[userA][language] && speak[userB][language]) {
                    hasCommonLanguage = true
                    break
                }
            }
            if (!hasCommonLanguage) {
                for (language in 1..n) {
                    if (!speak[userA][language]) {
                        teach[userA][language] = true
                    }
                    if (!speak[userB][language]) {
                        teach[userB][language] = true
                    }
                }
                listToTeach.add(friend)
            }
        }
        var minLanguage: Int = Int.MAX_VALUE
        var languageToTeach = 0
        for (language in 1..n) {
            var count = 0
            for (user in 1..m) {
                if (teach[user][language]) {
                    count++
                }
            }
            if (count < minLanguage) {
                minLanguage = count
                languageToTeach = language
            }
        }
        val setToTeach: MutableSet<Int> = HashSet()
        for (friend: IntArray in listToTeach) {
            val userA: Int = friend[0]
            val userB: Int = friend[1]
            if (!speak[userA][languageToTeach]) {
                setToTeach.add(userA)
            }
            if (!speak[userB][languageToTeach]) {
                setToTeach.add(userB)
            }
        }
        return setToTeach.size
    }
}
```