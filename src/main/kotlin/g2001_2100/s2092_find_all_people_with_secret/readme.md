[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2092\. Find All People With Secret

Hard

You are given an integer `n` indicating there are `n` people numbered from `0` to `n - 1`. You are also given a **0-indexed** 2D integer array `meetings` where <code>meetings[i] = [x<sub>i</sub>, y<sub>i</sub>, time<sub>i</sub>]</code> indicates that person <code>x<sub>i</sub></code> and person <code>y<sub>i</sub></code> have a meeting at <code>time<sub>i</sub></code>. A person may attend **multiple meetings** at the same time. Finally, you are given an integer `firstPerson`.

Person `0` has a **secret** and initially shares the secret with a person `firstPerson` at time `0`. This secret is then shared every time a meeting takes place with a person that has the secret. More formally, for every meeting, if a person <code>x<sub>i</sub></code> has the secret at <code>time<sub>i</sub></code>, then they will share the secret with person <code>y<sub>i</sub></code>, and vice versa.

The secrets are shared **instantaneously**. That is, a person may receive the secret and share it with people in other meetings within the same time frame.

Return _a list of all the people that have the secret after all the meetings have taken place._ You may return the answer in **any order**.

**Example 1:**

**Input:** n = 6, meetings = \[\[1,2,5],[2,3,8],[1,5,10]], firstPerson = 1

**Output:** [0,1,2,3,5]

**Explanation:**

At time 0, person 0 shares the secret with person 1.

At time 5, person 1 shares the secret with person 2.

At time 8, person 2 shares the secret with person 3.

At time 10, person 1 shares the secret with person 5.

Thus, people 0, 1, 2, 3, and 5 know the secret after all the meetings. 

**Example 2:**

**Input:** n = 4, meetings = \[\[3,1,3],[1,2,2],[0,3,3]], firstPerson = 3

**Output:** [0,1,3]

**Explanation:**

At time 0, person 0 shares the secret with person 3.

At time 2, neither person 1 nor person 2 know the secret.

At time 3, person 3 shares the secret with person 0 and person 1.

Thus, people 0, 1, and 3 know the secret after all the meetings. 

**Example 3:**

**Input:** n = 5, meetings = \[\[3,4,2],[1,2,1],[2,3,1]], firstPerson = 1

**Output:** [0,1,2,3,4]

**Explanation:**

At time 0, person 0 shares the secret with person 1.

At time 1, person 1 shares the secret with person 2, and person 2 shares the secret with person 3.

Note that person 2 can share the secret at the same time as receiving it.

At time 2, person 3 shares the secret with person 4.

Thus, people 0, 1, 2, 3, and 4 know the secret after all the meetings. 

**Constraints:**

*   <code>2 <= n <= 10<sup>5</sup></code>
*   <code>1 <= meetings.length <= 10<sup>5</sup></code>
*   `meetings[i].length == 3`
*   <code>0 <= x<sub>i</sub>, y<sub>i</sub> <= n - 1</code>
*   <code>x<sub>i</sub> != y<sub>i</sub></code>
*   <code>1 <= time<sub>i</sub> <= 10<sup>5</sup></code>
*   `1 <= firstPerson <= n - 1`

## Solution

```kotlin
@Suppress("NAME_SHADOWING")
class Solution {
    fun findAllPeople(n: Int, meetings: Array<IntArray>, firstPerson: Int): List<Int> {
        meetings.sortWith { a: IntArray, b: IntArray -> a[2] - b[2] }
        val uf = UF(n)
        // base
        uf.union(0, firstPerson)
        // for every time we have a pool of people that talk to each other
        // if someone knows a secret proir to this meeting - all pool will too
        // if not - reset unions from this pool
        var i = 0
        while (i < meetings.size) {
            val curTime = meetings[i][2]
            val pool: MutableSet<Int> = HashSet()
            while (i < meetings.size && curTime == meetings[i][2]) {
                val currentMeeting = meetings[i]
                uf.union(currentMeeting[0], currentMeeting[1])
                pool.add(currentMeeting[0])
                pool.add(currentMeeting[1])
                i++
            }
            // meeting that took place now should't affect future
            // meetings if people don't know the secret
            for (j in pool) {
                if (!uf.connected(0, j)) {
                    uf.reset(j)
                }
            }
        }
        // if the person is conneted to 0 - they know a secret
        val ans: MutableList<Int> = ArrayList()
        for (j in 0 until n) {
            if (uf.connected(j, 0)) {
                ans.add(j)
            }
        }
        return ans
    }

    // regular union find
    private class UF(n: Int) {
        private val parent: IntArray
        private val rank: IntArray

        init {
            parent = IntArray(n)
            rank = IntArray(n)
            for (i in 0 until n) {
                parent[i] = i
            }
        }

        fun union(p: Int, q: Int) {
            val rootP = find(p)
            val rootQ = find(q)
            if (rootP == rootQ) {
                return
            }
            if (rank[rootP] < rank[rootQ]) {
                parent[rootP] = rootQ
            } else {
                parent[rootQ] = rootP
                rank[rootP]++
            }
        }

        fun find(p: Int): Int {
            var p = p
            while (parent[p] != p) {
                p = parent[parent[p]]
            }
            return p
        }

        fun connected(p: Int, q: Int): Boolean {
            return find(p) == find(q)
        }

        fun reset(p: Int) {
            parent[p] = p
            rank[p] = 0
        }
    }
}
```