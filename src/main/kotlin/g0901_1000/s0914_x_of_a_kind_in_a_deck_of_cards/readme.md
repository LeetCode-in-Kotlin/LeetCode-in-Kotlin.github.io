[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Kotlin?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Kotlin?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin/fork)

## 914\. X of a Kind in a Deck of Cards

Easy

You are given an integer array `deck` where `deck[i]` represents the number written on the <code>i<sup>th</sup></code> card.

Partition the cards into **one or more groups** such that:

*   Each group has **exactly** `x` cards where `x > 1`, and
*   All the cards in one group have the same integer written on them.

Return `true` _if such partition is possible, or_ `false` _otherwise_.

**Example 1:**

**Input:** deck = [1,2,3,4,4,3,2,1]

**Output:** true

**Explanation:**: Possible partition [1,1],[2,2],[3,3],[4,4].

**Example 2:**

**Input:** deck = [1,1,1,2,2,2,3,3]

**Output:** false

**Explanation:**: No possible partition.

**Constraints:**

*   <code>1 <= deck.length <= 10<sup>4</sup></code>
*   <code>0 <= deck[i] < 10<sup>4</sup></code>

## Solution

```kotlin
class Solution {
    fun hasGroupsSizeX(deck: IntArray): Boolean {
        val map: HashMap<Int, Int> = HashMap()
        for (j in deck) {
            if (map.containsKey(j)) {
                map[j] = map.getValue(j) + 1
            } else {
                map[j] = 1
            }
        }
        var x = map.getValue(deck[0])
        for (entry in map.entries.iterator()) {
            x = gcd(x, entry.value)
        }
        return x >= 2
    }

    private fun gcd(a: Int, b: Int): Int {
        return if (b == 0) {
            a
        } else {
            gcd(b, a % b)
        }
    }
}
```