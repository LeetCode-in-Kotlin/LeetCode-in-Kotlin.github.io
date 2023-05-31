[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 433\. Minimum Genetic Mutation

Medium

A gene string can be represented by an 8-character long string, with choices from `'A'`, `'C'`, `'G'`, and `'T'`.

Suppose we need to investigate a mutation from a gene string `startGene` to a gene string `endGene` where one mutation is defined as one single character changed in the gene string.

*   For example, `"AACCGGTT" --> "AACCGGTA"` is one mutation.

There is also a gene bank `bank` that records all the valid gene mutations. A gene must be in `bank` to make it a valid gene string.

Given the two gene strings `startGene` and `endGene` and the gene bank `bank`, return _the minimum number of mutations needed to mutate from_ `startGene` _to_ `endGene`. If there is no such a mutation, return `-1`.

Note that the starting point is assumed to be valid, so it might not be included in the bank.

**Example 1:**

**Input:** startGene = "AACCGGTT", endGene = "AACCGGTA", bank = ["AACCGGTA"]

**Output:** 1

**Example 2:**

**Input:** startGene = "AACCGGTT", endGene = "AAACGGTA", bank = ["AACCGGTA","AACCGCTA","AAACGGTA"]

**Output:** 2

**Constraints:**

*   `0 <= bank.length <= 10`
*   `startGene.length == endGene.length == bank[i].length == 8`
*   `startGene`, `endGene`, and `bank[i]` consist of only the characters `['A', 'C', 'G', 'T']`.

## Solution

```kotlin
import java.util.LinkedList
import java.util.Queue

class Solution {
    private fun isInBank(set: Set<String>, cur: String): List<String> {
        val res: MutableList<String> = ArrayList()
        for (each in set) {
            var diff = 0
            for (i in 0 until each.length) {
                if (each[i] != cur[i]) {
                    diff++
                    if (diff > 1) {
                        break
                    }
                }
            }
            if (diff == 1) {
                res.add(each)
            }
        }
        return res
    }

    fun minMutation(start: String, end: String, bank: Array<String>): Int {
        val set: MutableSet<String> = HashSet()
        for (s in bank) {
            set.add(s)
        }
        val queue: Queue<String> = LinkedList()
        queue.offer(start)
        var step = 0
        while (queue.isNotEmpty()) {
            var curSize = queue.size
            while (curSize-- > 0) {
                val cur = queue.poll()
                if (cur == end) {
                    return step
                }
                for (next in isInBank(set, cur)) {
                    queue.offer(next)
                    set.remove(next)
                }
            }
            step++
        }
        return -1
    }
}
```