[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 1583\. Count Unhappy Friends

Medium

You are given a list of `preferences` for `n` friends, where `n` is always **even**.

For each person `i`, `preferences[i]` contains a list of friends **sorted** in the **order of preference**. In other words, a friend earlier in the list is more preferred than a friend later in the list. Friends in each list are denoted by integers from `0` to `n-1`.

All the friends are divided into pairs. The pairings are given in a list `pairs`, where <code>pairs[i] = [x<sub>i</sub>, y<sub>i</sub>]</code> denotes <code>x<sub>i</sub></code> is paired with <code>y<sub>i</sub></code> and <code>y<sub>i</sub></code> is paired with <code>x<sub>i</sub></code>.

However, this pairing may cause some of the friends to be unhappy. A friend `x` is unhappy if `x` is paired with `y` and there exists a friend `u` who is paired with `v` but:

*   `x` prefers `u` over `y`, and
*   `u` prefers `x` over `v`.

Return _the number of unhappy friends_.

**Example 1:**

**Input:** n = 4, preferences = \[\[1, 2, 3], [3, 2, 0], [3, 1, 0], [1, 2, 0]], pairs = \[\[0, 1], [2, 3]]

**Output:** 2

**Explanation:**

Friend 1 is unhappy because:

- 1 is paired with 0 but prefers 3 over 0, and

- 3 prefers 1 over 2.

Friend 3 is unhappy because:

- 3 is paired with 2 but prefers 1 over 2, and

- 1 prefers 3 over 0.

Friends 0 and 2 are happy.

**Example 2:**

**Input:** n = 2, preferences = \[\[1], [0]], pairs = \[\[1, 0]]

**Output:** 0

**Explanation:** Both friends 0 and 1 are happy.

**Example 3:**

**Input:** n = 4, preferences = \[\[1, 3, 2], [2, 3, 0], [1, 3, 0], [0, 2, 1]], pairs = \[\[1, 3], [0, 2]]

**Output:** 4

**Constraints:**

*   `2 <= n <= 500`
*   `n` is even.
*   `preferences.length == n`
*   `preferences[i].length == n - 1`
*   `0 <= preferences[i][j] <= n - 1`
*   `preferences[i]` does not contain `i`.
*   All values in `preferences[i]` are unique.
*   `pairs.length == n/2`
*   `pairs[i].length == 2`
*   <code>x<sub>i</sub> != y<sub>i</sub></code>
*   <code>0 <= x<sub>i</sub>, y<sub>i</sub> <= n - 1</code>
*   Each person is contained in **exactly one** pair.

## Solution

```kotlin
@Suppress("UNUSED_PARAMETER")
class Solution {
    fun unhappyFriends(n: Int, preferences: Array<IntArray>, pairs: Array<IntArray>): Int {
        var unhappyFriends = 0
        val assignedPair: MutableMap<Int, Int> = HashMap()
        for (pair in pairs) {
            assignedPair[pair[0]] = pair[1]
            assignedPair[pair[1]] = pair[0]
        }
        for (pair in pairs) {
            if (isUnHappy(pair[1], pair[0], preferences, assignedPair)) {
                unhappyFriends++
            }
            if (isUnHappy(pair[0], pair[1], preferences, assignedPair)) {
                unhappyFriends++
            }
        }
        return unhappyFriends
    }

    private fun isUnHappy(
        self: Int,
        assignedFriend: Int,
        preferences: Array<IntArray>,
        assignedPairs: Map<Int, Int>
    ): Boolean {
        val preference = preferences[self]
        val assignedFriendPreferenceIndex = findIndex(preference, assignedFriend)
        for (i in 0..assignedFriendPreferenceIndex) {
            val preferredFriend = preference[i]
            val preferredFriendAssignedFriend = assignedPairs[preferredFriend]!!
            if (preferredFriendAssignedFriend == self) {
                return false
            }
            val candidateAssignedFriendIndex = findIndex(preferences[preferredFriend], preferredFriendAssignedFriend)
            if (isPreferred(self, preferences[preferredFriend], candidateAssignedFriendIndex)) {
                return true
            }
        }
        return false
    }

    private fun isPreferred(self: Int, preference: IntArray, boundary: Int): Boolean {
        for (i in 0..boundary) {
            if (self == preference[i]) {
                return true
            }
        }
        return false
    }

    private fun findIndex(preference: IntArray, assignedFriend: Int): Int {
        for (i in preference.indices) {
            if (preference[i] == assignedFriend) {
                return i
            }
        }
        return 0
    }
}
```