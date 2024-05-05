[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 2251\. Number of Flowers in Full Bloom

Hard

You are given a **0-indexed** 2D integer array `flowers`, where <code>flowers[i] = [start<sub>i</sub>, end<sub>i</sub>]</code> means the <code>i<sup>th</sup></code> flower will be in **full bloom** from <code>start<sub>i</sub></code> to <code>end<sub>i</sub></code> (**inclusive**). You are also given a **0-indexed** integer array `persons` of size `n`, where `persons[i]` is the time that the <code>i<sup>th</sup></code> person will arrive to see the flowers.

Return _an integer array_ `answer` _of size_ `n`_, where_ `answer[i]` _is the **number** of flowers that are in full bloom when the_ <code>i<sup>th</sup></code> _person arrives._

**Example 1:**

![](https://assets.leetcode.com/uploads/2022/03/02/ex1new.jpg)

**Input:** flowers = \[\[1,6],[3,7],[9,12],[4,13]], persons = [2,3,7,11]

**Output:** [1,2,2,2]

**Explanation:** The figure above shows the times when the flowers are in full bloom and when the people arrive. For each person, we return the number of flowers in full bloom during their arrival.

**Example 2:**

![](https://assets.leetcode.com/uploads/2022/03/02/ex2new.jpg)

**Input:** flowers = \[\[1,10],[3,3]], persons = [3,3,2]

**Output:** [2,2,1]

**Explanation:** The figure above shows the times when the flowers are in full bloom and when the people arrive. For each person, we return the number of flowers in full bloom during their arrival.

**Constraints:**

*   <code>1 <= flowers.length <= 5 * 10<sup>4</sup></code>
*   `flowers[i].length == 2`
*   <code>1 <= start<sub>i</sub> <= end<sub>i</sub> <= 10<sup>9</sup></code>
*   <code>1 <= persons.length <= 5 * 10<sup>4</sup></code>
*   <code>1 <= persons[i] <= 10<sup>9</sup></code>

## Solution

```kotlin
import java.util.PriorityQueue

class Solution {
    fun fullBloomFlowers(flowers: Array<IntArray>, persons: IntArray): IntArray {
        flowers.sortWith { a: IntArray, b: IntArray -> a[0].compareTo(b[0]) }
        val ans = IntArray(persons.size)
        val pq = PriorityQueue { a: Pair, b: Pair -> a.j.compareTo(b.j) }
        var j = 0
        val t = Array(persons.size) { IntArray(2) }
        for (i in persons.indices) {
            t[i][0] = persons[i]
            t[i][1] = i
        }
        t.sortWith { a: IntArray, b: IntArray -> a[0].compareTo(b[0]) }
        for (ints in t) {
            while (pq.isNotEmpty()) {
                if (pq.peek().j < ints[0]) {
                    pq.poll()
                } else {
                    break
                }
            }
            while (j < flowers.size) {
                if (flowers[j][0] <= ints[0] && flowers[j][1] >= ints[0]) {
                    pq.add(Pair(flowers[j][0], flowers[j][1]))
                    j++
                    continue
                }
                if (flowers[j][1] < ints[0]) {
                    j++
                    continue
                }
                break
            }
            ans[ints[1]] = pq.size
        }
        return ans
    }

    private class Pair(var i: Int, var j: Int)
}
```