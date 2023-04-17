[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 911\. Online Election

Medium

You are given two integer arrays `persons` and `times`. In an election, the <code>i<sup>th</sup></code> vote was cast for `persons[i]` at time `times[i]`.

For each query at a time `t`, find the person that was leading the election at time `t`. Votes cast at time `t` will count towards our query. In the case of a tie, the most recent vote (among tied candidates) wins.

Implement the `TopVotedCandidate` class:

*   `TopVotedCandidate(int[] persons, int[] times)` Initializes the object with the `persons` and `times` arrays.
*   `int q(int t)` Returns the number of the person that was leading the election at time `t` according to the mentioned rules.

**Example 1:**

**Input** ["TopVotedCandidate", "q", "q", "q", "q", "q", "q"] [[[0, 1, 1, 0, 0, 1, 0], [0, 5, 10, 15, 20, 25, 30]], [3], [12], [25], [15], [24], [8]]

**Output:** [null, 0, 1, 1, 0, 0, 1]

**Explanation:** 
    
    TopVotedCandidate topVotedCandidate = new TopVotedCandidate([0, 1, 1, 0, 0, 1, 0], [0, 5, 10, 15, 20, 25, 30]); 
    topVotedCandidate.q(3); // return 0, At time 3, the votes are [0], and 0 is leading. 
    topVotedCandidate.q(12); // return 1, At time 12, the votes are [0,1,1], and 1 is leading. 
    topVotedCandidate.q(25); // return 1, At time 25, the votes are [0,1,1,0,0,1], and 1 is leading (as ties go to the most recent vote.) 
    topVotedCandidate.q(15); // return 0 
    topVotedCandidate.q(24); // return 0 
    topVotedCandidate.q(8); // return 1

**Constraints:**

*   `1 <= persons.length <= 5000`
*   `times.length == persons.length`
*   `0 <= persons[i] < persons.length`
*   <code>0 <= times[i] <= 10<sup>9</sup></code>
*   `times` is sorted in a strictly increasing order.
*   <code>times[0] <= t <= 10<sup>9</sup></code>
*   At most <code>10<sup>4</sup></code> calls will be made to `q`.

## Solution

```kotlin
class TopVotedCandidate(persons: IntArray, private val times: IntArray) {
    private val winnersAtTimeT: IntArray = IntArray(times.size)

    init {
        val counterArray = IntArray(persons.size)
        var maxVote = 0
        var maxVotedPerson = 0
        for (i in persons.indices) {
            val person = persons[i]
            val voteCount = counterArray[person]
            if (voteCount + 1 >= maxVote) {
                maxVote = voteCount + 1
                maxVotedPerson = person
            }
            winnersAtTimeT[i] = maxVotedPerson
            counterArray[persons[i]] = voteCount + 1
        }
    }

    fun q(t: Int): Int {
        var lo = 0
        var hi = times.size - 1
        if (t >= times[hi]) {
            lo = hi
        } else {
            while (lo < hi - 1) {
                val mid = lo + (hi - lo) / 2
                if (times[mid] == t) {
                    lo = mid
                    break
                } else if (times[mid] > t) {
                    hi = mid
                } else {
                    lo = mid
                }
            }
        }
        return winnersAtTimeT[lo]
    }
}

/**
 * Your TopVotedCandidate object will be instantiated and called as such:
 * var obj = TopVotedCandidate(persons, times)
 * var param_1 = obj.q(t)
 */
```