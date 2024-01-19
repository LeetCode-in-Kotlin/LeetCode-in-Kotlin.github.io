[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2983\. Palindrome Rearrangement Queries

Hard

You are given a **0-indexed** string `s` having an **even** length `n`.

You are also given a **0-indexed** 2D integer array, `queries`, where <code>queries[i] = [a<sub>i</sub>, b<sub>i</sub>, c<sub>i</sub>, d<sub>i</sub>]</code>.

For each query `i`, you are allowed to perform the following operations:

*   Rearrange the characters within the **substring** <code>s[a<sub>i</sub>:b<sub>i</sub>]</code>, where <code>0 <= a<sub>i</sub> <= b<sub>i</sub> < n / 2</code>.
*   Rearrange the characters within the **substring** <code>s[c<sub>i</sub>:d<sub>i</sub>]</code>, where <code>n / 2 <= c<sub>i</sub> <= d<sub>i</sub> < n</code>.

For each query, your task is to determine whether it is possible to make `s` a **palindrome** by performing the operations.

Each query is answered **independently** of the others.

Return _a **0-indexed** array_ `answer`_, where_ `answer[i] == true` _if it is possible to make_ `s` _a palindrome by performing operations specified by the_ <code>i<sup>th</sup></code> _query, and_ `false` _otherwise._

*   A **substring** is a contiguous sequence of characters within a string.
*   `s[x:y]` represents the substring consisting of characters from the index `x` to index `y` in `s`, **both inclusive**.

**Example 1:**

**Input:** s = "abcabc", queries = \[\[1,1,3,5],[0,2,5,5]]

**Output:** [true,true]

**Explanation:** In this example, there are two queries:

In the first query:

- a<sub>0</sub> = 1, b<sub>0</sub> = 1, c<sub>0</sub> = 3, d<sub>0</sub> = 5.

- So, you are allowed to rearrange s[1:1] => a<ins>b</ins>cabc and s[3:5] => abc<ins>abc</ins>.

- To make s a palindrome, s[3:5] can be rearranged to become => abc<ins>cba</ins>.

- Now, s is a palindrome. So, answer[0] = true.

In the second query:

- a<sub>1</sub> = 0, b<sub>1</sub> = 2, c<sub>1</sub> = 5, d<sub>1</sub> = 5.

- So, you are allowed to rearrange s[0:2] => <ins>abc</ins>abc and s[5:5] => abcab<ins>c</ins>.

- To make s a palindrome, s[0:2] can be rearranged to become => <ins>cba</ins>abc.

- Now, s is a palindrome. So, answer[1] = true. 

**Example 2:**

**Input:** s = "abbcdecbba", queries = \[\[0,2,7,9]]

**Output:** [false]

**Explanation:** In this example, there is only one query.

a<sub>0</sub> = 0, b<sub>0</sub> = 2, c<sub>0</sub> = 7, d<sub>0</sub> = 9.

So, you are allowed to rearrange s[0:2] => <ins>abb</ins>cdecbba and s[7:9] => abbcdec<ins>bba</ins>.

It is not possible to make s a palindrome by rearranging these substrings because s[3:6] is not a palindrome.

So, answer[0] = false.

**Example 3:**

**Input:** s = "acbcab", queries = \[\[1,2,4,5]]

**Output:** [true]

**Explanation:** In this example, there is only one query.

a<sub>0</sub> = 1, b<sub>0</sub> = 2, c<sub>0</sub> = 4, d<sub>0</sub> = 5.

So, you are allowed to rearrange s[1:2] => a<ins>cb</ins>cab and s[4:5] => acbc<ins>ab</ins>.

To make s a palindrome s[1:2] can be rearranged to become a<ins>bc</ins>cab.

Then, s[4:5] can be rearranged to become abcc<ins>ba</ins>.

Now, s is a palindrome. So, answer[0] = true.

**Constraints:**

*   <code>2 <= n == s.length <= 10<sup>5</sup></code>
*   <code>1 <= queries.length <= 10<sup>5</sup></code>
*   `queries[i].length == 4`
*   <code>a<sub>i</sub> == queries[i][0], b<sub>i</sub> == queries[i][1]</code>
*   <code>c<sub>i</sub> == queries[i][2], d<sub>i</sub> == queries[i][3]</code>
*   <code>0 <= a<sub>i</sub> <= b<sub>i</sub> < n / 2</code>
*   <code>n / 2 <= c<sub>i</sub> <= d<sub>i</sub> < n</code>
*   `n` is even.
*   `s` consists of only lowercase English letters.

## Solution

```kotlin
class Solution {
    private var n = 0

    // get associated index in the other half
    private fun opp(i: Int): Int {
        return n - 1 - i
    }

    fun canMakePalindromeQueries(s: String, queries: Array<IntArray>): BooleanArray {
        val fq = IntArray(26)
        val m = queries.size
        val ret = BooleanArray(m)
        n = s.length
        // check that both halves contain the same letters
        for (i in 0 until n / 2) {
            fq[s[i].code - 'a'.code]++
        }
        for (i in n / 2 until n) {
            fq[s[i].code - 'a'.code]--
        }
        for (em in fq) {
            if (em != 0) {
                return ret
            }
        }
        // find the first and the last characters in the first half
        // that do not match with their associated character in
        // the second half
        var problemPoint = -1
        var lastProblem = -1
        for (i in 0 until n / 2) {
            if (s[i] != s[opp(i)]) {
                if (problemPoint == -1) {
                    problemPoint = i
                }
                lastProblem = i
            }
        }
        // if already a palindrome
        if (problemPoint == -1) {
            ret.fill(true)
            return ret
        }
        // the idea is that at least one of the intervals in the
        // query has to cover the first pair of different characters.
        // But depending on how far the other end of that interval
        // goes, the requirements for the other interval are lessened
        val dpFirst = IntArray(n / 2 + 1)
        val dpSecond = IntArray(n + 1)
        dpFirst.fill(-1)
        dpSecond.fill(-1)
        // assuming the first interval covers the first problem,
        // and then extends to the right
        var rptr = opp(problemPoint)
        val mp: MutableMap<Char, Int> = HashMap()
        for (i in problemPoint until n / 2) {
            mp.compute(s[i]) { _: Char?, v: Int? -> if (v == null) 1 else v + 1 }
            // the burden for the left end of the second interval does not change;
            // it needs to go at least until the last problematic match. But the
            // requirements for the right end do. If we can rearrange the characters
            // in the left half to match the right end of the right interval, this
            // means we do not need the right end of the right interval to go too far
            while (mp.containsKey(s[rptr]) ||
                (rptr >= n / 2 && s[rptr] == s[opp(rptr)] && mp.size == 0)
            ) {
                mp.computeIfPresent(s[rptr]) { _: Char?, v: Int -> if (v == 1) null else v - 1 }
                rptr--
            }
            dpFirst[i] = rptr
        }
        // mirrored discussion assuming it is the right interval that takes
        // care of the first problematic pair
        var lptr = problemPoint
        mp.clear()
        for (i in opp(problemPoint) downTo n / 2) {
            mp.compute(s[i]) { _: Char?, v: Int? -> if (v == null) 1 else v + 1 }
            while (mp.containsKey(s[lptr]) ||
                (lptr < n / 2 && s[lptr] == s[opp(lptr)] && mp.size == 0)
            ) {
                mp.computeIfPresent(s[lptr]) { _: Char?, v: Int -> if (v == 1) null else v - 1 }
                lptr++
            }
            dpSecond[i] = lptr
        }
        for (i in 0 until m) {
            val a = queries[i][0]
            val b = queries[i][1]
            val c = queries[i][2]
            val d = queries[i][3]
            // if either interval the problematic interval on its side, it does not matter
            // what happens with the other interval
            if (a <= problemPoint && b >= lastProblem ||
                c <= opp(lastProblem) && d >= opp(problemPoint)
            ) {
                ret[i] = true
                continue
            }
            // if the left interval covers the first problem, we use
            // dp to figure out if the right one is large enough
            if (a <= problemPoint && b >= problemPoint && d >= dpFirst[b] && c <= opp(lastProblem)) {
                ret[i] = true
            }
            // similarly for the case where the right interval covers
            // the first problem
            if (d >= opp(problemPoint) && c <= opp(problemPoint) && a <= dpSecond[c] && b >= lastProblem) {
                ret[i] = true
            }
        }
        return ret
    }
}
```